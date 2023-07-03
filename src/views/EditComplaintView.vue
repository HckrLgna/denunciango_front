<template>
    <div class="editComplaint">
      <Header/>
      <b-row>
          <SidebarMenu :isMenuOpen="true" profileImg="" />
        <b-col class="px-0">
            <EditComplaint/>
        </b-col>
      </b-row>
         
    </div>
      
    </template>
    
    <script>
    import SidebarMenu from "../components/SidebarMenu";
    import Header from '@/components/Header.vue';
    import EditComplaint from "@/components/EditComplaint.vue";
    export default {
      name: "ImboxView",
      components: { SidebarMenu, Header, EditComplaint},
      data() {
        return {};
      },
    };
    </script>
    
    <style scoped>
   
    </style>