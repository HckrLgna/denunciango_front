<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="6" >
                <h2>Administrar area</h2>
            </b-col>

            <b-col col md="3" >
                <b-pagination
                    v-model="currentPage"
                    :total-rows="rows"
                    :per-page="perPage"
                    aria-controls="my-table"
                ></b-pagination>
            </b-col>
            <b-col col md="3" >
                <b-button :to="{name: 'user.register'}">Agregar Area</b-button>
            </b-col>
            
        </b-row>
        <b-row>
            <div class="overflow-auto"> 
            <b-table
                id="my-table"
                :items="items"
                :fields="fields"
                :per-page="perPage"
                :current-page="currentPage"
                medium
            >
             
            <template v-slot:cell(ver)="row">
                <router-link :to="`/map-complaint/${row.item.id}`">Ver </router-link>
            </template>

            </b-table>
            </div>
        </b-row>
    </b-container>
   
  </template>
  
  <script>
import axios from 'axios'
    export default {
      data() {
        return {
          perPage: 3,
          currentPage: 1,
          items: [
            {id: 'areId' , nombre: 'areNombre', descripcion: 'areDesc', areCelular: 'areCelular ', areDireccion: 'mapa'},
           
          ],
          fields: [
                { key: 'id', label: 'ID' },
                { key: 'areNombre', label: 'Nombres' },
                { key: 'areDesc', label: 'descripcion' },
                { key: 'areCelular', label: 'Area' },
                { key: 'areDireccion', label: 'Direccion'  },
                
            ],
          value: '',
          
          isHidden: true,
          selected: [],
        }
      },
      mounted() {
            axios.get('http://denunciangows.fly.dev/api/areObtenerAreas')
                .then(response=>{
                    const areas = response.data.data;
                    this.items = areas;
                })
        },
      methods: {
         
      },
      computed: {
        rows() {
                return this.items.length
                }
      },

    }
  </script>
  <style>
     
  </style>