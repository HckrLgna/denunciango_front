<template>
    <div class="imbox">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
            <AreaRegister/>
        </b-col>
      </b-row>
    </div>
  </template>
    
    <script>
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/HeaderAuth.vue';
    import AreaRegister from '@/components/AreaRegister.vue';
 
    export default {
      name: "UsersView",
      components: { SidebarMenu, Header, AreaRegister},
      data() {
        return {};
      },
    };
    </script>
    
    <style >
   
    </style>