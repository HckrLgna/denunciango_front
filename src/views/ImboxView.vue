<template>
  <div class="imbox">
    <Header/>
    <SidebarMenu :isMenuOpen="true" profileImg="" />
      <div>
        <ImboxTable/>
      </div>
  </div>
    
  </template>
  
  <script>
  import SidebarMenu from "../components/SidebarMenu";
  import Header from '@/components/Header.vue';
  import ImboxTable from '@/components/ImboxTable.vue'
  
  export default {
    name: "ImboxView",
    components: { SidebarMenu, Header, ImboxTable},
    data() {
      return {};
    },
  };
  </script>
  
  <style scoped>
 
  </style>