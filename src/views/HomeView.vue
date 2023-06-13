<template>
  <div class="home">
    <Header/>
    <Carrusel></carrusel>
      <form-filtros/>
    <maps/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue';
import Carrusel from '../components/Carrusel.vue';
import Maps from '../components/Maps.vue';
import FormFiltros from '../components/FormFiltros.vue';

export default {
  name: 'HomeView',
  components: {
    Header,
    Carrusel,
    Maps,
    FormFiltros
  }
}
</script>
