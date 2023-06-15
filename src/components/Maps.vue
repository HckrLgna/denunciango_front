<template>
  <div class="p-5">
    <b-container>
      <div class="mb-3">
        <b-form-group
          id="input-group-3"
          label="Tipo de denuncia:"
          label-for="input-3"
        >
          <b-form-select
            id="input-tipo"
            :options="tipos"
          ></b-form-select>
          <b-form-select
            id="input-estado"
            v-model="form.food"
            :options="estados"
          ></b-form-select>

          <div class="mb-3">
            <b-form-datepicker
              v-model="filterFechaInicio"
              placeholder="Fecha de inicio"
            ></b-form-datepicker>
            <b-form-datepicker
              v-model="filterFechaFin"
              placeholder="Fecha de fin"
            ></b-form-datepicker>
          </div>
        </b-form-group>
      </div>

      
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
      form: {
        email: "",
        name: "",
        food: null,
        checked: [],
      },
      tipos: [],
      estados: [],
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
        const { denLat, denLng, denTdTitulo, denTitulo } = marker;
        const position = new google.maps.LatLng(
          parseFloat(denLat),
          parseFloat(denLng)
        );

        return new google.maps.Marker({
          position,
          map,
          label: denTdTitulo,
          title: denTitulo,
        });
      });

      new MarkerClusterer(map, googleMarkers, { imagePath: imgClusterUrl });
    },
    fetchMarkers() {
      const apiUrl = "http://cuidomivoto.com/api/obtenerDenuncias";
      axios
        .get(apiUrl)
        .then((response) => {
          const denuncias = response.data.data.denuncias;

          const { ests, tds } = response.data.data;

          this.tipos = tds.map(td => td.tdTitulo);
          this.estados = ests.map(est => est.estTitulo);
          
          this.locations.markers = denuncias.map((denuncia) => ({
            denId: denuncia.denId,
            denLat: denuncia.denLat,
            denLng: denuncia.denLng,
            denTdTitulo: denuncia.denTdTitulo,
            denTitulo: denuncia.denTitulo,
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
  },
};
</script>
  
  <style>
.google-map {
  width: 1024;
  height: 500px;
}
</style>
  