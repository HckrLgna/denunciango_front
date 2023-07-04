<template>
    <div class="map-complaint">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
            <maps/>
        </b-col>
      </b-row>
     </div>
  </template>
  
  <script>
  // @ is an alias to /src
  import Header from '@/components/Header.vue';
  import SidebarMenu from "../components/SidebarMenu";
  import Maps from '../components/Maps.vue';
  
  export default {
    name: 'MapComplaintView',
    components: {
      Header,
      Maps,
      SidebarMenu
    }
  }
  </script>
  