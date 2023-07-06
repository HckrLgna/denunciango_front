<template>
    <div class="map">
      <Header/>
      <maps/>
      <Footer/>
    </div>
  </template>
  
  <script>
  // @ is an alias to /src
  import Header from '@/components/Header.vue';
  import Maps from '../components/Maps.vue';
  import Footer from '../components/Footer.vue';
  
  export default {
    name: 'MapView',
    components: {
      Header,
      Maps,
      Footer,
    }
  }
  </script>
  