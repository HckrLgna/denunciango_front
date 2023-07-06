<template>
  <div  >
    <b-container fluid class="bg-light py-3 px-5 ">
      <b-row>
        <b-row class="px-3 p-2 mx-3">
          <h3>Mapa de denuncias</h3>
        </b-row>

          <b-col class="col col-md-3 mx-5 p-4 align-items-center custom-bg">
            <h5>Filtros</h5>
            <b-col class="p-1 w-100">
              <b-form-group class="w-100" 
                id="input-group-estado"
                label="Seleccione un estado:"
                label-for="input-estado"
              >
                <b-form-select class="w-100" 
                  id="input-estado"
                  v-model="estado"
                  :options="estados"
                  @change="handleEstadoChange"
                ></b-form-select>
              </b-form-group>
            </b-col>
            <b-col class="p-1 w-100">
              <b-form-group class="w-100" 
                id="input-group-tipo"
                label="Seleccione un tipo:"
                label-for="input-tipo"
              >
                <b-form-select class="w-100" 
                  id="input-tipo"
                  v-model="tipo"
                  :options="tipos"
                  @change="handleTipoChange"
                ></b-form-select>
              </b-form-group>
            </b-col>
            <b-col class="p-1 w-100">
              <b-form-group class="w-100" 
                id="input-group-fechaIni"
                label="Seleccione una fecha inicio:"
                label-for="input-fechaIni">
                <b-form-datepicker class="w-100" 
                  id="input-fechaIni"
                  v-model="filterFechaInicio"
                  placeholder="Fecha de inicio"
                  @change="handleFechaInicioChange"
                ></b-form-datepicker>
              </b-form-group>
            
            </b-col>
            <b-col class="p-1 w-100">
              <b-form-group class="w-100" 
                id="input-group-fechaFin"
                label="Seleccione una fecha final:"
                label-for="input-fechaFin">
                <b-form-datepicker class="w-100" 
                  id="input-fechaFin"
                  v-model="filterFechaFin"
                  placeholder="Fecha fin"
                  @change="handleFechaFinChange"
                ></b-form-datepicker>
              </b-form-group>
            </b-col>
        
            <b-col class="w-100 py-3" >
              <b-form-checkbox
                  id="checkbox-1"
                  v-model="status"
                  name="checkbox-1"
                  value="accepted"
                  unchecked-value="not_accepted"
                  @change="handleStatusChange"
                >
                  Aplicar filtros
                </b-form-checkbox>
            </b-col>
          </b-col> 


        <b-col class="col">
          <b-row class="px-5">
            <div ref="googleMap" class="google-map"></div>
          </b-row>
        </b-col>
      </b-row>
        

      
     
    </b-container>
  </div>
</template>
  
<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      mapOptions: {
        center: { lat: -17.7831936, lng: -63.1701504 },
        zoomControl: true,
        zoom: 13,
        gestureHandling: "cooperative",
      },
      locations: {
        imgClusterUrl:
          "https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m",
        markers: [],
      },
      
      filterFechaInicio: null, // Fecha de inicio seleccionada para filtrar
      filterFechaFin: null,
      estado: null,
      tipo: null, 
      tipos: [],
      estados: [],
      status: false,
      accepted: false,
    };
  },
  mounted() {
    this.initMap();
    this.fetchMarkers();
    this.getLocation();
  },
  methods: {
    initMap() {
      const { imgClusterUrl, markers } = this.locations;
      const map = new google.maps.Map(this.$refs.googleMap, {
        ...this.mapOptions,
      });
      const infoWindow = new google.maps.InfoWindow();
      const googleMarkers = markers.map((marker) => {
        const { denLat, denLng, denTdTitulo, denTitulo, denTdTipo ,denFecha, denEstTitulo} = marker;
        const position = new google.maps.LatLng(
          parseFloat(denLat),
          parseFloat(denLng)
        );
        const markerIcons = {
          1: 'https://maps.google.com/mapfiles/ms/icons/green-dot.png',
          2: 'https://maps.google.com/mapfiles/ms/icons/purple-dot.png',
          3: 'https://maps.google.com/mapfiles/ms/icons/red-dot.png',
          4: 'https://maps.google.com/mapfiles/ms/icons/blue-dot.png',
        };
        
        const icon = markerIcons[denTdTipo] || null;

        const googleMarker = new google.maps.Marker({
          position,
          map,
          label: denTdTitulo,
          title: denTitulo,
          icon: icon,
        });

        // Agregar evento click al marcador para mostrar el infoWindow
        googleMarker.addListener("click", () => {
          const content = `
            <div>
              <h5>${denTdTitulo}</h5>
              <p>${denTitulo}</p>
              <p>Estado: ${denEstTitulo}</p>
              <p>Fecha: ${denFecha}</p>
            </div>
          `;
          infoWindow.setContent(content);
          infoWindow.open(map, googleMarker);
        });

        return googleMarker;

      });

      new MarkerClusterer(map, googleMarkers, { imagePath: imgClusterUrl });
    },
    mapTipos(tds) {
      this.tipos = [{ value: "null", text: "Todos" }, ...tds.map(td => ({ value: td.tdTitulo, text: td.tdTitulo }))];
    },
    mapEstados(ests) {
      this.estados = [{ value: "null", text: "Todos" }, ...ests.map(est => ({ value: est.estTitulo, text: est.estTitulo }))];
    },
    fetchMarkers() {
      const apiUrl = "http://cuidomivoto.com/api/obtenerDenuncias";
      axios
        .get(apiUrl)
        .then((response) => {
          const denuncias = response.data.data.denuncias;

          const { ests, tds } = response.data.data;
          console.log('carga fetch markers')
          this.mapTipos(tds);
          this.mapEstados(ests);
          const filteredMarkers = denuncias.filter((denuncia) => {
        // Filtrar por tipo de denuncia
            if (this.tipo && this.tipo !== "null" && denuncia.denTdTitulo !== this.tipo) {
              return false;
            }

            // Filtrar por estado
            if (this.estado && this.estado !== "null" && denuncia.denEstTitulo !== this.estado) {
              return false;
            }

            // Filtrar por rango de fechas
            if (this.status && this.filterFechaInicio && this.filterFechaFin) {
              const fechaInicio = new Date(this.filterFechaInicio);
              const fechaFin = new Date(this.filterFechaFin);
              const fechaDenuncia = new Date(denuncia.denFecha);
               
              if (fechaDenuncia < fechaInicio || fechaDenuncia > fechaFin) {
                return false;

              }
            }

            return true;
          });

          this.locations.markers = filteredMarkers.map((denuncia) => ({
            denId: denuncia.denId,
            denLat: denuncia.denLat,
            denLng: denuncia.denLng,
            denTdTitulo: denuncia.denTdTitulo,
            denTitulo: denuncia.denTitulo,
            denTdTipo: denuncia.denTipo,
            denEstTitulo: denuncia.denEstTitulo,
            denFecha: denuncia.denFecha
          }));
          this.initMap();
          
        })
        .catch((error) => {
          console.error(error);
        });
    },
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition);
      } else {
        console.log("La geolocalización no es soportada por este navegador.");
      }
    },
    showPosition(position) {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;
      this.latActual = latitude;
      this.lngActual = longitude;
      console.log(latitude);
      console.log(longitude);
    },
    handleEstadoChange() {
      this.fetchMarkers();
    },
      // Método para manejar el evento de cambio de valor del selector de tipo
    handleTipoChange() {
      this.fetchMarkers();
    },

    // Método para manejar el evento de selección de fecha de inicio
    handleFechaInicioChange() {
      this.fetchMarkers();
    },

    // Método para manejar el evento de selección de fecha de fin
    handleFechaFinChange() {
      this.fetchMarkers();
    },
    handleStatusChange(){
      console.log(this.status == "accepted");
      if(this.status != "accepted"){
        this.limpiarDatos();
      }
      this.fetchMarkers();
     
    },
    limpiarDatos(){
      console.log("entro limpiar datos")
      this.status=false;
      this.tipo=null;
      this.estado=null;
      this.filterFechaFin=null;
      this.filterFechaInicio=null;
    },
     
  },
};
</script>
  
<style>
  .google-map {
    width: 1024;
    height: 500px;
  }

  .custom-bg {
        background-color: #eed7f5; /* Color de fondo personalizado */
  }
  
  .custom-bg2 {
      background-color: #3a044b; /* Color de fondo personalizado */
  }
</style>