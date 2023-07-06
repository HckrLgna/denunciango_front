<template>
    <div>
      <b-carousel
        id="carousel-1"
        v-model="slide"
        :interval="4000"
        controls
        indicators
        background="#ababab"
        img-width="860"
        img-height="560"
        style="text-shadow: 1px 1px 2px #333;"
        @sliding-start="onSlideStart"
        @sliding-end="onSlideEnd"
      >
      
        <!-- Text slides with image -->
        <b-carousel-slide>
        <!-- <b-carousel-slide
          caption="Bienvenido al portal de denuncias del"
          text="GOBIERNO AUTONOMO MUNICIPAL DE SANTA CRUZ DE LA SIERRA"
        > -->
        <h4>Bienvenido al portal de denuncias del</h4>
        <h3>GOBIERNO AUTONOMO MUNICIPAL DE SANTA CRUZ DE LA SIERRA</h3>
        <h5>Solidario, moderno y participativo</h5>
        <template #img>
            <img
              class="d-block  "
              width="2048"
              height="430"
              src="../assets/portada2.jpg"
              alt="image slot"
            >
          </template>
      </b-carousel-slide>
      
      <!-- Slides with custom text -->
      <b-carousel-slide>
          <h4>Bienvenido al portal de denuncias del</h4>
          <h3>GOBIERNO AUTONOMO MUNICIPAL DE SANTA CRUZ DE LA SIERRA</h3>
          <h5>Solidario, moderno y participativo</h5>
          <template #img>
            <img
              class="d-block  "
              width="2048"
              height="430"
              src="../assets/portada1.jpg"
              alt="image slot"
            >
             
          </template>
          
        </b-carousel-slide>
  
        <!-- Slides with image only -->  
        <!-- Slides with img slot -->
        <!-- Note the classes .d-block and .img-fluid to prevent browser default image alignment -->
        <b-carousel-slide>
          <h4>Bienvenido al portal de denuncias del</h4>
          <h3>GOBIERNO AUTONOMO MUNICIPAL DE SANTA CRUZ DE LA SIERRA</h3>
          <h5>Solidario, moderno y participativo</h5>
          <template #img>
            <img
              class="d-block  "
              width="2048"
              height="430"
              src="../assets/image 12.png"
              alt="image slot"
            >
          </template>
        </b-carousel-slide>
  
        
      </b-carousel>
  
       
    </div>
  </template>
  
  <script>
    export default {
      name:'Carrusel',
      data() {
        return {
          slide: 0,
          sliding: null
        }
      },
      methods: {
        onSlideStart(slide) {
          this.sliding = true
        },
        onSlideEnd(slide) {
          this.sliding = false
        }
      }
    }
  </script>