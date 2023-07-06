<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="9" >
                <h2>Bandeja de Entrada > Ver denuncia</h2>
            </b-col>

            
        </b-row>
        <b-row>
            <b-col col md="5" class="p-3">
                <b-row>
                    <b-col col md="6">
                        <b-img thumbnail fluid :src="'data:image/jpeg;base64,' + imgDen1" alt="Image 1"></b-img>
                    </b-col >
                    <b-col col md="6" v-if="imgDen2 != null ">
                        <b-img thumbnail fluid :src="'data:image/jpeg;base64,' + imgDen2" alt="Image 2"></b-img>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col class="p-5">
                        <b-card
                            border-variant="secondary"
                            :header="'Denunciado por: ' + paterno + materno + nombre"
                            header-border-variant="secondary"
                            align="center"
                        >
                            <div class="image-container">
                                <b-img class="img-fluid" :src="'data:image/jpeg;base64,' + perfil" rounded="circle" alt="Image 2"></b-img>
                            </div>
                            <b-card-text> Correo: {{ correoUsuario }}</b-card-text>
                            <b-card-text>Direccion: {{ direccion }}</b-card-text>
                            <b-card-text>Cedula identidad: {{ ci }}</b-card-text>
                            
                        </b-card>
                    </b-col>
                </b-row>
            </b-col>
            <b-col col md="7" class="p-3">
                <b-form @submit="onSubmit"  v-if="show">
                 <b-row>
                    <b-form-group
                    id="input-group-titulo"
                    label="Titulo"
                    label-for="input-titulo"
                    >
                    <b-form-input
                    id="input-titulo"
                    v-model="form.titulo"
                    type="text"
                    placeholder=""
                    value="asd"
                    required
                    ></b-form-input>
                    </b-form-group>

                    <b-form-group id="input-group-descripcion" label="Descripcion" label-for="textarea-descripcion">
                        <b-form-textarea
                            id="textarea-descripcion"
                            placeholder="Fixed height textarea"
                            v-model="form.descripcion"
                            rows="3"
                            no-resize
                        ></b-form-textarea>
                    </b-form-group>
                 </b-row>
                 <b-row>
                    <b-col>
                        <label for="fecha-datepicker">Fecha:</label>
                        <b-form-datepicker id="fecha-datepicker" v-model="form.fecha" class="mb-2"></b-form-datepicker>

                        <b-form-group id="input-group-estado" label="Estado" label-for="select-estado">
                            <b-form-select
                            id="select-estado"
                            v-model="form.estado"
                            :options="estados"
                            required
                            ></b-form-select>
                        </b-form-group>
                    </b-col>
                    <b-col class="py-4">
                        <b-button @click="openModal">Ver mapa</b-button>
                        <b-modal v-model="modalShow" @shown="initMap">
                            <b-col class="col ">
                                <b-row class="px-5">
                                    <div ref="googleMap" class="google-map"></div>
                                </b-row>
                            </b-col>
                        </b-modal>

                        <b-form-group id="input-group-tipo" label="Tipo" label-for="select-tipo">
                            <b-form-select
                            id="select-tipo"
                            v-model="form.tipo"
                            :options="tipos"
                            ></b-form-select>
                        </b-form-group>
                        
                    </b-col>
                 </b-row>   
                 <b-row>
                    <b-form-group
                    id="input-group-comentario"
                    label="Comentario de respuesta"
                    label-for="textarea-comentario"
                    >
                        <b-form-textarea
                            id="textarea-comentario"
                            placeholder="ingresa un comentario"
                            v-model="form.comentario"
                            rows="3"
                            no-resize
                        ></b-form-textarea>
                    </b-form-group>
                    
                    <b-form-group class="p-3">
                         <b-button type="submit" variant="primary">Aceptar</b-button>
                        <b-button type="reset" variant="danger">Rechazar</b-button>
                    </b-form-group>
                       
                 </b-row>

                </b-form>
            </b-col>
        </b-row>
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

            form: {
                titulo:'',
                descripcion:'',
                fecha:null,
                ubicacion:'',
                estado: null,
                tipo: null,
                comentario:''
            },
            estados:[],
            tipos:[],
            imgDen1: '',
            imgDen2: '',

            correoUsuario: '',
            paterno: '',
            materno: '',
            nombre: '',
            direccion: '',
            perfil: '',
            ci:'',

            show: true,
            comentario:'',
            //modal 
            modalShow: false
        }
      },
      mounted() {
        axios.get('https://denunciangows.fly.dev/api/obtenerDenuncias')
            .then(response => {
                this.estados = response.data.data.ests.map(est => ({ value: est.estId, text: est.estTitulo }));
                this.tipos = response.data.data.tds.map(td => ({ value: td.tdId, text: td.tdTitulo }));
            })
            .catch(error => {
                console.error(error);
        });

        this.fetchDetalleDenuncia();
      },
      computed: {
        
      },
      methods:{
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
            
            const position = new google.maps.LatLng(
                parseFloat(this.locations.markers.denLat),
                parseFloat(this.locations.markers.denLng)
            );
            
            const marker = new google.maps.Marker({
                position,
                map,
                icon: 'https://maps.google.com/mapfiles/ms/icons/red-dot.png',
            });

            // Agregar evento click al marcador para mostrar el infoWindow
            marker.addListener("click", () => {
                const content = `
                <div>
                    <h5>Título del marcador</h5>
                    <p>Otra información del marcador</p>
                </div>
                `;
                infoWindow.setContent(content);
                infoWindow.open(map, marker);
            });

            new MarkerClusterer(map, [marker], { imagePath: imgClusterUrl });
        },
        onSubmit(event){
            console.log('eviando')
        },
        fetchDetalleDenuncia(){
            const denId = this.$route.params.id;
            axios.post('https://denunciangows.fly.dev/api/obtenerDetDen/', { denId }, {
            headers: {
                'Content-Type': 'application/json'
                }
            })
            .then(response => {
                // Aquí puedes manejar la respuesta de la solicitud POST
                const denuncias = response.data.data.den;
                
                this.form.titulo = response.data.data.den.denTitulo
                this.form.descripcion = response.data.data.den.denDescripcion
                const fecha = new Date(response.data.data.den.denFecha);
                this.form.fecha = fecha;
 

                const estadoSeleccionado = this.estados.find(
                    estado => estado.value === response.data.data.den.denEstado
                );

                if (estadoSeleccionado) {
                    this.form.estado = estadoSeleccionado.value;
                }
                const tipoSeleccionado = this.tipos.find(
                    tipo => tipo.value === response.data.data.den.denTipo
                );
                console.log(tipoSeleccionado);
                if (tipoSeleccionado) {    
                    this.form.tipo = tipoSeleccionado.value;
                }

                this.imgDen1 = response.data.data.denImagenes[0];
                this.imgDen2 = response.data.data.denImagenes[1];
                this.correoUsuario = response.data.data.den.denUsu;
                this.fetchDetalleDenunciante();
                    this.locations.markers = {
                        denLat: denuncias.denLat,
                        denLng: denuncias.denLng,}
                    }

            )
            .catch(error => {
                console.error(error);
            });
        },
        fetchDetalleDenunciante(){ 
            console.log(this.correoUsuario);
            const usuEmail = this.correoUsuario;
            axios.post('https://denunciangows.fly.dev/api/propietarioDen', { usuEmail }, {
            headers: {
                'Content-Type': 'application/json'
                }
            })
            .then(response =>{
                const denunciante = response.data.data;
                console.log(response.data.data);
                this.paterno = denunciante.usuPaterno;
                this.materno = denunciante.usuMaterno;
                this.nombre = denunciante.usuNombre;
                this.ci  = denunciante.usuCI;
                this.direccion = denunciante.usuDireccion;
                this.perfil =   denunciante.usuFoto;

            })
            .catch(error => {

            });
        }
      }
    }
  </script>
<style>
    .google-map {
        width: 2024px;
        height: 500px;
    }
    .image-container {
    max-width: 150px; /* Ajusta el tamaño máximo deseado */
    height: auto; /* Ajusta la altura automáticamente */
    margin: 0 auto; /* Centra el contenedor horizontalmente */
    }
</style>