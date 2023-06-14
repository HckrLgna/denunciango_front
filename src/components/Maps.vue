<template>
    <div class="p-5">
        <b-container >
            <div ref="googleMap" class="google-map"></div>
        </b-container>
      
    </div>
  </template>
  
  <script>
  export default {
    name: 'App',
    data() {
      return {
        mapOptions: {
          center: { lat: -17.824069, lng: -63.130153 },
          zoomControl: true,
          zoom: 17,
          gestureHandling: 'cooperative',
        },
        locations: {
          imgClusterUrl:
            'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m',
          locations: [
            {
              id: 1,
              lat: -17.824076,
              lng: -63.127887,
              name_point: 'A',
              title: 'Alcantarillado',
            },
            {
              id: 2,
              lat: -17.825486,
              lng: -63.127951,
              name_point: 'B',
              title: 'Basura ',
            },
            {
              id: 3,
              lat: -17.826303,
              lng: -63.130827,
              name_point: 'C',
              title: 'Calles y avenidas',
            },
            
          ],
        },
      };
    },
    mounted() {
      this.initMap();
    },
    methods: {
      initMap() {
        const { imgClusterUrl, locations } = this.locations;
        const map = new google.maps.Map(this.$refs.googleMap, {
          ...this.mapOptions,
        });
        let markers = locations.map((location) => {
          const setLocations = new google.maps.LatLng(location.lat, location.lng);
          return new google.maps.Marker({
            position: setLocations,
            map: map,
            label: location.name_point,
            title: location.title,
          });
        });
        new MarkerClusterer(map, markers, { imagePath: imgClusterUrl });
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
  