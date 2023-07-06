<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
    <CardsHome />

    <div class="mx-5 px-3 my-4 justify-content-between">
      <h3>¿Que es denunciango app?</h3>
      <p class="fs-5">Es una herramienta fácil y accesible para informar y reportar diferentes tipos de incidentes o situaciones problemáticas que ocurran en la ciudad de Santa Cruz.</p>
      <img src="../assets/descarga.png" class="w-100" fluid alt="Responsive image">
    </div>

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
