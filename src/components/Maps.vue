<template>
  <div class="p-5">
    <b-container>
        <form  >
          <b-row md="8" class="p-3">
            <b-col md="3">
              <b-form-group
              id="input-group-estado"
              label="Seleccione un estado:"
              label-for="input-estado">
                <b-form-select
                  id="input-estado"
                  v-model="estado"
                  :options="estados"
                  size="sm"
                  @change="handleEstadoChange"
                ></b-form-select>
              </b-form-group>
              
            </b-col>  
            <b-col md="3">
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
            <b-col md="3">
              <b-form-datepicker
                v-model="filterFechaInicio"
                placeholder="Fecha de inicio"
                @change="handleFechaInicioChange"
              ></b-form-datepicker>
            </b-col>
            <b-col md="3">
              <b-form-datepicker
                v-model="filterFechaFin"
                placeholder="Fecha de fin"
                @change="handleFechaFinChange"
              ></b-form-datepicker>
            </b-col>
           
          </b-row>
          <b-row md="12">
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
 
        </form>

      
      <div ref="googleMap" class="google-map"></div>
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
        center: { lat: -17.824069, lng: -63.130153 },
        zoomControl: true,
        zoom: 17,
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

      const googleMarkers = markers.map((marker) => {
        const { denLat, denLng, denTdTitulo, denTitulo, denTdTipo } = marker;
        const position = new google.maps.LatLng(
          parseFloat(denLat),
          parseFloat(denLng)
        );
        const markerIcons = {
          1: 'https://maps.google.com/mapfiles/ms/icons/red-dot.png',
          2: 'https://maps.google.com/mapfiles/ms/icons/green-dot.png',
          3: 'https://maps.google.com/mapfiles/ms/icons/blue-dot.png',
          4: 'https://maps.google.com/mapfiles/ms/icons/purple-dot.png',
        };

        const icon = markerIcons[denTdTipo] || null;

        return new google.maps.Marker({
          position,
          map,
          label: denTdTitulo,
          title: denTitulo,
          icon: icon
        });
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
            denTdTipo: denuncia.denTipo
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
  