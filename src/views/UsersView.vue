<template>
  <div class="imbox">
    <Header/>
    <b-row>
      <SidebarMenu :isMenuOpen="true" profileImg="" />
      <b-col>
        <UserTable/>
      </b-col>
    </b-row>
  </div>
</template>
  
  <script>
  import SidebarMenu from "../components/SidebarMenu";
  import Header from '@/components/Header.vue';
  import UserTable from "@/components/UserTable.vue";
  
  export default {
    name: "UsersView",
    components: { SidebarMenu, Header, UserTable},
    data() {
      return {};
    },
  };
  </script>
  
  <style scoped>
 
  </style>