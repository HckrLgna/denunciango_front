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
                <b-dropdown id="dropdown-form" text="Dropdown with form" ref="dropdown" class="m-2">
                    <b-dropdown-form>
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
                        <b-form-group
                            id="input-group-fechaIni"
                            label="Seleccione una fecha inicio:"
                            label-for="input-fechaIni">
                            <b-form-datepicker id="example-datepicker" v-model="value" class="mb-2"></b-form-datepicker>
                        </b-form-group>
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
            { descripcion: 'El parque del km 6 esta descuidado', fecha: '23/06/2023-11:59', categoria: 'Parques y jardines', estado: 'nueva', editar: 'editar', ver: 'mapa'},
           
          ],
          value: '',
          filterFechaInicio: null, // Fecha de inicio seleccionada para filtrar
          filterFechaFin: null,
          estado: null,
          tipo: null, 
          tipos: [],
          estados: [],
        }
      },
      computed: {
        rows() {
          return this.items.length
        }
      }
    }
  </script>