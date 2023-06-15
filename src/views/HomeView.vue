<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
    <maps/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';
import Maps from '../components/Maps.vue';

export default {
  name: 'HomeView',
  components: {
    Header,
    Carrusel,
    Maps,
  }
}
</script>
