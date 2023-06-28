<template>
    <div class="map">
      <Header/>
      <maps/>
    </div>
  </template>
  
  <script>
  // @ is an alias to /src
  import Header from '@/components/Header.vue';
  import Maps from '../components/Maps.vue';
  
  export default {
    name: 'MapView',
    components: {
      Header,
      Maps,
    }
  }
  </script>
  