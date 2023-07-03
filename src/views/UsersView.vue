<template>
  <div class="imbox">
    <Header/>
    <SidebarMenu :isMenuOpen="true" profileImg="" />
      <div>
 
      </div>
  </div>
    
  </template>
  
  <script>
  import SidebarMenu from "../components/SidebarMenu";
  import Header from '@/components/Header.vue';
  
  export default {
    name: "UsersView",
    components: { SidebarMenu, Header},
    data() {
      return {};
    },
  };
  </script>
  
  <style scoped>
 
  </style>