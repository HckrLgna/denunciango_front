<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="6" >
                <h2>Administrar usuario</h2>
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
                <b-button :to="{name: 'user.register'}">Agregar usuario</b-button>
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