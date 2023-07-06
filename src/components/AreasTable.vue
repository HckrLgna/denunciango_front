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
            {id:1 , apellidos: 'El parque del km 6 esta descuidado', nombres: '23/06/2023-11:59', area: 'Parques y jardines', ver: 'mapa'},
           
          ],
          fields: [
                { key: 'id', label: 'ID' },
                { key: 'apellidos', label: 'Apellidos' },
                { key: 'nombres', label: 'Nombres' },
                { key: 'area', label: 'Area' },
                { key: 'ver', label: 'Ver', sortable: false  },
            ],
          value: '',
          
          isHidden: true,
          selected: [],
        }
      },
      mounted() {
            axios
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