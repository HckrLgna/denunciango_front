<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="6" >
                <h2>Administrar Tipos de denuncias</h2>
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
                <b-button :to="{name: 'type.register'}">Agregar tipo de denuncia</b-button>
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
            { nombre: 'tdTitulo', descripcion: 'tdDesc', areCelular: 'tdEstado ' },
           
          ],
          fields: [
                
                { key: 'tdTitulo', label: 'Titulo' },
                { key: 'tdDesc', label: 'Descripcion' },
                { key: 'tdEstado', label: 'Estado' },
                
            ],
          value: '',
          
          isHidden: true,
          selected: [],
        }
      },
      mounted() {
            axios.get('https://denunciangows.fly.dev/api/tiposDenuncia')
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