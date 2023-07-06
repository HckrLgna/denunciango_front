<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="5" >
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
                <b-row>
                    <b-col col md="6">
                        <b-dropdown id="dropdown-form" text="Filtros"  block ref="dropdown" class="m-2" onclick="event.stopPropagation()">
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

                                <b-dropdown-item class="dropdownItem" style="min-width: 318px;">
                                    <b-form-datepicker id="customDatePicker" size="sm" v-model="filterFechaInicio"></b-form-datepicker>
                                    <div id="customCalendarIni" v-if="!isHidden">
                                    <b-calendar v-model="filterFechaInicio"></b-calendar>
                                    </div>
                                </b-dropdown-item>

                                <b-dropdown-item class="dropdownItem" style="min-width: 318px;">
                                    <b-form-datepicker id="customDatePicker" size="sm" v-model="filterFechaFin"></b-form-datepicker>
                                    <div id="customCalendarFin" v-if="!isHidden">
                                    <b-calendar v-model="filterFechaFin"></b-calendar>
                                    </div>
                                </b-dropdown-item>
                                <b-form-checkbox
                                    id="checkbox-1"
                                    v-model="status"
                                    name="checkbox-1"
                                    value="accepted"
                                    unchecked-value="not_accepted"
                                    @change="handleStatusChange"
                                >
                                    Aplicar filtros
                                </b-form-checkbox>
                            </b-dropdown-form> 
                         </b-dropdown>
                    </b-col>
                    <b-col col md="6">
                        <b-button v-b-modal.modal-xl @click="openModal">Ver seleccion</b-button>
                        <b-modal size="xl" v-model="modalShow" @shown="initMap">
                            <b-col class="col ">
                                <b-row class="px-5">
                                    <div ref="googleMap" class="google-map"></div>
                                </b-row>
                            </b-col>
                        </b-modal>
                    </b-col>
                </b-row>
                
            </b-col>
            
        </b-row>
        <b-row>
            <div class="overflow-auto px-3"> 
                <b-table
                    id="my-table"
                    :items="items"
                    :fields="fields"
                    :sort-by.sync="sortBy"
                    :sort-desc.sync="sortDesc"
                    :per-page="perPage"
                    :current-page="currentPage"
                    medium
                >
                    <template v-slot:cell(selected)="row">
                        <input type="checkbox" :checked="selected.includes(row.item.denId)" @input="toggleSelected(row.item.denId)">
                    </template>

                    <template v-slot:cell(editar)="row">
                        <router-link :to="`/complaint/${row.item.denId}`">Editar</router-link>
                    </template>
                </b-table>
            </div>
        </b-row>
        <div>
            Sorting By: <b>{{ sortBy }}</b>, Sort Direction:
            <b>{{ sortDesc ? 'Descending' : 'Ascending' }}</b>
        </div>
    </b-container>
   
  </template>
  
  <script>
import axios from 'axios';
    export default {
      data() {
        return {
            mapOptions: {
                center: { lat: -17.7831936, lng: -63.1701504 },
                zoomControl: true,
                zoom: 13,
                gestureHandling: "cooperative",
            },
            locations: {
                imgClusterUrl:
                "https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m",
                markers: [],
            },
          perPage: 13,
          currentPage: 1,
          items: [],
          fields: [
                { key: 'selected', label: 'Seleccionar', sortable: false},
                { key: 'denTitulo', label: 'Titulo', sortable: true },
                { key: 'denFecha', label: 'Fecha', sortable: true },
                { key: 'denTdTitulo', label: 'Tipo', sortable: false },
                { key: 'denEstTitulo', label: 'Estado', sortable: false},
                { key: 'editar', label: 'Editar',sortable: false },
                 
            ],
          value: '',
          filterFechaInicio: null, // Fecha de inicio seleccionada para filtrar
          filterFechaFin: null,
          status: false,
          estado: null,
          tipo: null, 
          tipos: [],
          estados: [],
          date: null,
          isHidden: true,
          selected: [],
          sortBy: 'denFecha',
          sortDesc: false,
          //modal 
          modalShow: false
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

            const customCalendarIni = document.getElementById('customCalendarIni');
            customCalendarIni?.addEventListener('click',(event)=>{
                event.stopPropagation();
            })
            const customCalendarFin = document.getElementById('customCalendarFin');
            customCalendarFin?.addEventListener('click',(event)=>{
                event.stopPropagation();
            })
            this.fetchMarkers();
               
         
        },
      methods: {
        openModal(){
            this.modalShow = true;
            this.initMap();
        },
        initMap() {
            const { imgClusterUrl, markers } = this.locations;
            const map = new google.maps.Map(this.$refs.googleMap, {
                ...this.mapOptions,
            });
            const infoWindow = new google.maps.InfoWindow();
            const googleMarkers = markers.map((marker) => {
                const { denLat, denLng, denTdTitulo, denTitulo, denTdTipo ,denFecha, denEstTitulo} = marker;
                const position = new google.maps.LatLng(
                parseFloat(denLat),
                parseFloat(denLng)
                );
                const markerIcons = {
                1: 'https://maps.google.com/mapfiles/ms/icons/green-dot.png',
                2: 'https://maps.google.com/mapfiles/ms/icons/purple-dot.png',
                3: 'https://maps.google.com/mapfiles/ms/icons/red-dot.png',
                4: 'https://maps.google.com/mapfiles/ms/icons/blue-dot.png',
                };

                const icon = markerIcons[denTdTipo] || null;

                const googleMarker = new google.maps.Marker({
                    position,
                    map,
                    label: denTdTitulo,
                    title: denTitulo,
                    icon: icon,
                });

                // Agregar evento click al marcador para mostrar el infoWindow
                googleMarker.addListener("click", () => {
                const content = `
                    <div>
                    <h5>${denTdTitulo}</h5>
                    <p>${denTitulo}</p>
                    <p>Estado: ${denEstTitulo}</p>
                    <p>Fecha: ${denFecha}</p>
                    </div>
                `;
                infoWindow.setContent(content);
                infoWindow.open(map, googleMarker);
                });

                return googleMarker;

            });

            new MarkerClusterer(map, googleMarkers, { imagePath: imgClusterUrl });
        },
        fetchMarkers(){
            axios.get('https://denunciangows.fly.dev/api/obtenerDenuncias')
            .then(response => {
            // Manejar la respuesta del servidor y asignar los datos a la variable "items"
            const denuncias = response.data.data.denuncias;
            this.showSelectComplaint(denuncias);
            const { ests, tds } = response.data.data;
            console.log(response.data.data.denuncias);
            this.items = response.data.data.denuncias;
            this.mapTipos(tds);
            this.mapEstados(ests);
            
            const filteredMarkers = denuncias.filter((denuncia) => {
            // Filtrar por tipo de denuncia
                if (this.tipo && this.tipo !== "null" && denuncia.denTdTitulo !== this.tipo) {
                    return false;
                }

                // Filtrar por estado
                if (this.estado && this.estado !== "null" && denuncia.denEstTitulo !== this.estado) {
                    return false;
                }

                // Filtrar por rango de fechas
                if (this.status && this.filterFechaInicio && this.filterFechaFin) {
                const fechaInicio = new Date(this.filterFechaInicio);
                const fechaFin = new Date(this.filterFechaFin);
                const fechaDenuncia = new Date(denuncia.denFecha);
                
                    if (fechaDenuncia < fechaInicio || fechaDenuncia > fechaFin) {
                        return false;

                    }
                }
                return true;
                });
                
                this.items = filteredMarkers;
            })
            .catch(error => {
            // Manejar el error de la solicitud
            console.error(error);
            });
           
        },
        showSelectComplaint(denuncias){
            const idArray = Object.values(this.selected).map(item => item);
            console.log(idArray);
            
            const filterComplaint = denuncias.filter((denuncia) =>{
                return this.selected.includes(denuncia.denId);
            });
            console.log(filterComplaint);

            this.locations.markers = filterComplaint.map((denuncia) => ({
                    denId: denuncia.denId,
                    denLat: denuncia.denLat,
                    denLng: denuncia.denLng,
                    denTdTitulo: denuncia.denTdTitulo,
                    denTitulo: denuncia.denTitulo,
                    denTdTipo: denuncia.denTipo,
                    denEstTitulo: denuncia.denEstTitulo,
                    denFecha: denuncia.denFecha
                }));
        },
        mapTipos(tds) {
            this.tipos = [{ value: "null", text: "Todos" }, ...tds.map(td => ({ value: td.tdTitulo, text: td.tdTitulo }))];
        },
        mapEstados(ests) {
            this.estados = [{ value: "null", text: "Todos" }, ...ests.map(est => ({ value: est.estTitulo, text: est.estTitulo }))];
        },
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
            this.selected.join(',');
            this.fetchMarkers();  
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
    .google-map {
        width: 100%;
        height: 500px;
    }
  </style>