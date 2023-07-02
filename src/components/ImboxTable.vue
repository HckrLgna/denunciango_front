<template>
    <b-container class="p-5">
        <b-row>
            <b-col col md="7" >
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
            <b-col col md="2" >
                <b-dropdown id="dropdown-1" text="Dropdown Button" class="m-md-2">
                    <b-col class="col col-md-4">
                        <b-row class="px-5 py-4 align-items-center">
                            <b-col class="col-4 p-1 h-100">
                                <b-form-group
                                    id="input-group-estado"
                                    label=" estado:"
                                    label-for="input-estado"
                                >
                                    <b-form-select
                                    id="input-estado"
                                    v-model="estado"
                                    :options="estados"
                                    @change="handleEstadoChange"
                                    ></b-form-select>
                                </b-form-group>
                            </b-col>
                     
                            <b-col class="col-6 p-1 h-100">
                                <b-form-group
                                    id="input-group-fechaIni"
                                    label="Seleccione una fecha inicio:"
                                >
                                    <b-form-datepicker
                                    id="input-fechaIni"
                                    v-model="filterFechaInicio"
                                    @change="handleFechaInicioChange"
                                    ></b-form-datepicker>
                                </b-form-group>
                            </b-col>
                                <b-col class="col-6 p-1 h-100">
                            <b-form-group
                                id="input-group-fechaFin"
                                label="Seleccione una fecha final:"
                            >
                                <b-form-datepicker
                                id="input-fechaFin"
                                v-model="filterFechaFin"
                                @change="handleFechaFinChange"
                                ></b-form-datepicker>
                            </b-form-group>
                                </b-col>
                
                        </b-row> 
                 
                </b-col> 
                </b-dropdown>
            </b-col>
            
        </b-row>
        <b-row>
            <div class="overflow-auto"> 
            <b-table
                id="my-table"
                :items="items"
                :per-page="perPage"
                :current-page="currentPage"
                medium
            ></b-table>
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
            { id: 1, first_name: 'Fred', last_name: 'Flintstone' },
            { id: 2, first_name: 'Wilma', last_name: 'Flintstone' },
            { id: 3, first_name: 'Barney', last_name: 'Rubble' },
            { id: 4, first_name: 'Betty', last_name: 'Rubble' },
            { id: 5, first_name: 'Pebbles', last_name: 'Flintstone' },
            { id: 6, first_name: 'Bamm Bamm', last_name: 'Rubble' },
            { id: 7, first_name: 'The Great', last_name: 'Gazzoo' },
            { id: 8, first_name: 'Rockhead', last_name: 'Slate' },
            { id: 9, first_name: 'Pearl', last_name: 'Slaghoople' }
          ]
        }
      },
      computed: {
        rows() {
          return this.items.length
        }
      }
    }
  </script>