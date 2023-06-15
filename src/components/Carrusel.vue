<template>
    <div>
      <b-carousel
        id="carousel-1"
        v-model="slide"
        :interval="4000"
        controls
        indicators
        background="#ababab"
        img-width="1024"
        img-height="580"
        style="text-shadow: 1px 1px 2px #333;"
        @sliding-start="onSlideStart"
        @sliding-end="onSlideEnd"
      >
        <!-- Text slides with image -->
        <b-carousel-slide
          caption="First slide"
          text="Nulla vitae elit libero, a pharetra augue mollis interdum."
          img-src=" "
        ></b-carousel-slide>
  
        <!-- Slides with custom text -->
        <b-carousel-slide img-src=" ">
          <h2>Bienvenido al portal de denuncias del</h2>
          <h1>GOBIERNO AUTONOMO MUNICIPAL DE SANTA CRUZ DE LA SIERRA </h1>
          <h3>Solidario, moderno y participativo</h3>
        </b-carousel-slide>
  
        <!-- Slides with image only -->
        <b-carousel-slide img-src=" "></b-carousel-slide>
  
        <!-- Slides with img slot -->
        <!-- Note the classes .d-block and .img-fluid to prevent browser default image alignment -->
        <b-carousel-slide>
          <template #img>
            <img
              class="d-block img-fluid w-100"
              width="1024"
              height="380"
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