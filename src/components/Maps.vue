<template>
  <div  >
    <b-container fluid class="bg-white p-5 ">
      <b-row class="px-5 py-4">
        <h1>Categorias de denuncias</h1>
      </b-row>
      <b-row class="px-4">
 
          <b-col>
            <b-card
              bg-variant="success" text-variant="white" class="text-center"
              title="Area verde"
              img-src="https://picsum.photos/600/300/?image=25"
              img-alt="Image"
              img-top
              tag="article"
              style="max-width: 20rem;"
            >
              <b-card-text>
                "Área Verde" se refiere a la comunicación o reporte que se realiza para informar sobre una situación relacionada con un área o espacio destinado a la vegetación y la naturaleza.
              </b-card-text>

            </b-card>
          </b-col>
  
        
        <b-col>
          <b-card
            bg-variant="secondary" text-variant="white" class="text-center"
            title=" Via publica"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
          >
            <b-card-text>
              "Vía Pública" se refiere a cualquier reporte o queja relacionada con problemas, irregularidades o situaciones que afectan la calidad, el orden o la seguridad de las vías públicas de una determinada localidad. 
            </b-card-text>

          </b-card>
        </b-col>
        <b-col>
          <b-card
            bg-variant="danger" text-variant="white"   class="text-center"
            title="Servicio de Basura"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
          >
            <b-card-text>
               "Servicio de Basura" se refiere a situaciones relacionadas con la gestión inadecuada o deficiente del servicio de recolección y manejo de residuos sólidos en una determinada área o comunidad.
            </b-card-text>

          </b-card>
        </b-col>
        <b-col>
          <b-card 
            bg-variant="info" text-variant="white"  class="text-center"
            title="Alumbrado"
            img-src="https://picsum.photos/600/300/?image=25"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
          >
            <b-card-text>
              "Alumbrado" se refiere a un reporte o queja relacionada con el sistema de iluminación de una determinada área, como calles, parques, aceras u otros espacios públicos de recreacion parques y jardines.  
            </b-card-text>

          </b-card>
        </b-col>

      </b-row>
 
    </b-container>

    <b-container fluid class="bg-light p-5 ">
        <b-row class="px-5 py-2 mx-3">
          <h2>Visualiza las denuncias cercanas</h2>
        </b-row>
        <b-row class="px-5 py-4 align-items-center">
        <b-col class="col-4 p-1 h-100">
          <b-form-group
            id="input-group-estado"
            label="Seleccione un estado:"
            label-for="input-estado"
          >
            <b-form-select
              id="input-estado"
              v-model="estado"
              :options="estados"
              @change="handleEstadoChange"
            ></b-form-select>
          </b-form-group>
        </b-col>
        <b-col class="col-4 p-1 h-100">
          <b-form-group
            id="input-group-tipo"
            label="Seleccione un tipo:"
            label-for="input-tipo"
          >
            <b-form-select
              id="input-tipo"
              v-model="tipo"
              :options="tipos"
              @change="handleTipoChange"
            ></b-form-select>
          </b-form-group>
        </b-col>
        <b-col class="col-6 p-1 h-100">
          <b-form-group
            id="input-group-fechaIni"
            label="Seleccione una fecha inicio:"
            label-for="input-fechaIni">
            <b-form-datepicker
              id="input-fechaIni"
              v-model="filterFechaInicio"
              placeholder="Fecha de inicio"
              @change="handleFechaInicioChange"
            ></b-form-datepicker>
          </b-form-group>
         
        </b-col>
        <b-col class="col-6 p-1 h-100">
          <b-form-group
            id="input-group-fechaFin"
            label="Seleccione una fecha final:"
            label-for="input-fechaFin">
            <b-form-datepicker
              id="input-fechaFin"
              v-model="filterFechaFin"
              placeholder="Fecha fin"
              @change="handleFechaFinChange"
            ></b-form-datepicker>
          </b-form-group>
        </b-col>
    
      </b-row> 
      <b-row class="px-5 pb-5" >
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
      </b-row>

    

       <b-row class="px-5">
        <div ref="googleMap" class="google-map"></div>
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
</style>
  