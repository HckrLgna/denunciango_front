<template>
    <div class="imbox">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
          <AreasTable/>
        </b-col>
      </b-row>
    </div>
  </template>
    
<script>
    import AreasTable from "@/components/AreasTable.vue";
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/HeaderAuth.vue';
    
    export default {
      name: "UsersView",
      components: { SidebarMenu, Header, AreasTable},
      data() {
        return {};
      },
    };
    </script>
    
    <style scoped>
   
    </style>