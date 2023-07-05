<template>
  <div class="imbox">
    <Header/>
    <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
      <b-col class="px-0">
        <ImboxTable/>
      </b-col>
    </b-row>
       
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