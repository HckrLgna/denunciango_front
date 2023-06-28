<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';

export default {
  name: 'HomeView',
  components: {
    Header,
    Carrusel,
  }
}
</script>
