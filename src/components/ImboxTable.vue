<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="6" >
                <h2>Bandeja de Entrada</h2>
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
                <b-dropdown id="dropdown-form" text="Filtros"  block ref="dropdown" class="m-2" onclick="event.stopPropagation()">
                    <b-dropdown-form>
                        <b-form-group label="Ordenar" v-slot="{ ariaDescribedby }">
                            <b-form-radio v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios" value="A">Ascendente</b-form-radio>
                            <b-form-radio v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios" value="B">Descendente</b-form-radio>
                        </b-form-group>

                        <b-form-group
                            id="input-group-estado"
                            label="Seleccione un estado:"
                            label-for="input-estado"
                        >
                            <b-form-select
                            id="input-estado"
                            v-model="estado"
                            :options="estados"
                            @change="handleEstadoChange"
                            ></b-form-select>
                        </b-form-group>

                        <b-form-group
                            id="input-group-tipo"
                            label="Seleccione un tipo:"
                            label-for="input-tipo"
                        >
                            <b-form-select
                            id="input-tipo"
                            v-model="tipo"
                            :options="tipos"
                            @change="handleTipoChange"
                            ></b-form-select>
                        </b-form-group>
                        <b-dropdown-divider></b-dropdown-divider>

                        <b-dropdown-item class="dropdownItem" style="min-width: 318px;">
                            <b-form-datepicker id="customDatePicker" v-model="date"></b-form-datepicker>
                            <div id="customCalendar" v-if="!isHidden">
                            <b-calendar v-model="date"></b-calendar>
                            </div>
                        </b-dropdown-item>

                        <b-form-group
                            id="input-group-fechaFin"
                            label="Seleccione una fecha final:"
                            label-for="input-fechaFin">
                            <b-form-datepicker
                            id="input-fechaFin"
                            v-model="filterFechaFin"
                            placeholder="Fecha fin"
                            @change="handleFechaFinChange"
                            ></b-form-datepicker>
                        </b-form-group>
                        <b-form-checkbox class="mb-3">Remember me</b-form-checkbox>
                    </b-dropdown-form>
                
                </b-dropdown>
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
            <template v-slot:cell(selected)="row">
                <input type="checkbox" :checked="selected.includes(row.item.id)" @input="toggleSelected(row.item.id)">
            </template>
            <template v-slot:cell(editar)="row">
                <router-link :to="`/complaint/${row.item.denId}`">Editar</router-link>
            </template>
            <template v-slot:cell(ver)="row">
                <router-link :to="`/map-complaint/${row.item.denId}`">Ver mapa</router-link>
            </template>

            </b-table>
            </div>
        </b-row>
    </b-container>
   
  </template>
  
  <script>
import axios from 'axios';
    export default {
      data() {
        return {
          perPage: 13,
          currentPage: 1,
          items: [],
          fields: [
                { key: 'selected', label: 'Seleccionar'  },
                { key: 'denTitulo', label: 'Titulo' },
                { key: 'denFecha', label: 'Fecha' },
                { key: 'denTipo', label: 'Tipo' },
                { key: 'denEstTitulo', label: 'Estado' },
                { key: 'editar', label: 'Editar', sortable: false },
                { key: 'ver', label: 'Ver', sortable: false  },
            ],
          value: '',
          filterFechaInicio: null, // Fecha de inicio seleccionada para filtrar
          filterFechaFin: null,
          estado: null,
          tipo: null, 
          tipos: [],
          estados: [],
          date: null,
          isHidden: true,
          selected: [],
        }
      },
      mounted() {
        
            let dropdown = this.$refs.dropdown;
            const customDatePicker = document.getElementById('customDatePicker');
            customDatePicker?.addEventListener('click', (event) => {
                event.stopPropagation();
                this.isHidden = !this.isHidden;
                console.log(this.isHidden);
            });

            document.addEventListener("click", () => {
            this.isHidden = true;
            });

            const customCalendar = document.getElementById('customCalendar');
            customCalendar?.addEventListener('click',(event)=>{
                event.stopPropagation();
            })
            axios.get('https://denunciangows.fly.dev/api/obtenerDenuncias')
            .then(response => {
            // Manejar la respuesta del servidor y asignar los datos a la variable "items"
            console.log(response.data.data.denuncias);
            this.items = response.data.data.denuncias;
            })
            .catch(error => {
            // Manejar el error de la solicitud
            console.error(error);
            });
         
        },
      methods: {
        handleEstadoChange() {
            this.fetchMarkers();
        },
            // Método para manejar el evento de cambio de valor del selector de tipo
        handleTipoChange() {
            this.fetchMarkers();
        },

            // Método para manejar el evento de selección de fecha de inicio
        handleFechaInicioChange() {
            this.fetchMarkers();
        },
            // Método para manejar el evento de selección de fecha de fin
        handleFechaFinChange() {
            this.fetchMarkers();
        },
        handleStatusChange(){
            console.log(this.status == "accepted");
            if(this.status != "accepted"){
                this.limpiarDatos();
            }
            this.fetchMarkers();
            
        },
        limpiarDatos(){
            console.log("entro limpiar datos")
            this.status=false;
            this.tipo=null;
            this.estado=null;
            this.filterFechaFin=null;
            this.filterFechaInicio=null;
        },
        toggleSelected(id) {
            if (this.selected.includes(id)) {
            this.selected = this.selected.filter(item => item !== id);
            } else {
            this.selected.push(id);
            }
            console.log(this.selected.join(','));
        },
      },
      computed: {
        rows() {
                return this.items.length
                }
      },

    }
  </script>
  <style>
    #customDatePicker {
        width: 0;
        height: 0;
        padding: 0;
    }
    #customDatePicker > svg {
        padding-left: 10px;
        padding-right: 25px;
    }
    #customDatePicker__value_ {
        margin-left: 30px;
    }
  </style>