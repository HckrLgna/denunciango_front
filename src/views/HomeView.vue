<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
    <CardsHome />
    <Footer></Footer>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';
import Footer from '../components/Footer.vue';
import CardsHome from '../components/CardsHome.vue';

export default {
  name: 'HomeView',
  components: {
    Header,
    Carrusel,
    Footer,
    CardsHome,
  }
}
</script>
