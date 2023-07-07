<template>
    <div class="imbox">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
            <TypeComplaintRegister/>
        </b-col>
      </b-row>
    </div>
  </template>
    
    <script>
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/HeaderAuth.vue';
    import TypeComplaintRegister from '@/components/TypeComplaintRegister.vue';
 
    export default {
      name: "UsersView",
      components: { SidebarMenu, Header, TypeComplaintRegister},
      data() {
        return {};
      },
    };
    </script>
    
    <style >
   
    </style>