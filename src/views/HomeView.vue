<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';
import Footer from '../components/Footer.vue';

export default {
  name: 'HomeView',
  components: {
    Header,
    Carrusel,
    Footer,
  }
}
</script>
