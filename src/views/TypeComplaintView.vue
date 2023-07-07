<template>
    <div class="type">
      <Header/>
      <b-row>
        <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col>
          <TypeComplaintTable/>
        </b-col>
      </b-row>
    </div>
  </template>
    
<script>
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/HeaderAuth.vue';
    import TypeComplaintTable from "@/components/TypeComplaintTable.vue";
    export default {
      name: "UsersView",
      components: { SidebarMenu, Header, TypeComplaintTable},
      data() {
        return {};
      },
    };
    </script>
    
    <style scoped>
   
    </style>