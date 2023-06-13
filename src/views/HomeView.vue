<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    Header,
    Carrusel
  }
}
</script>
