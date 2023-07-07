<template>
    <div class="imbox">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
            <UserRegister/>
        </b-col>
      </b-row>
    </div>
  </template>
    
    <script>
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/HeaderAuth.vue';
    import UserRegister from "@/components/UserRegister.vue";
    export default {
      name: "UsersView",
      components: { SidebarMenu, Header, UserRegister},
      data() {
        return {};
      },
    };
    </script>
    
    <style scoped>
   
    </style>