<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="9" >
                <h2>Bandeja de Entrada > Ver denuncia</h2>
            </b-col>

            <b-col col md="3" >
                <b-button>Guardar Cambios</b-button>
            </b-col>
        </b-row>
        <b-row>
            <b-col col md="4">
                <b-row>
                    <b-col>
                        <b-img thumbnail fluid :src="'data:image/jpeg;base64,' + imgDen1" alt="Image 1"></b-img>
                    </b-col>
                    <b-col v-if="imgDen2 != null ">
                        <b-img thumbnail fluid :src="'data:image/jpeg;base64,' + imgDen2" alt="Image 2"></b-img>
                    </b-col>
                </b-row>
                <b-row>
                    <b-col>
                        <b-card
                            border-variant="secondary"
                            header="Denunciado por:"
                            header-border-variant="secondary"
                            align="center"
                        >
                            <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=58" rounded="circle" alt="Image 2"></b-img>
                            <b-card-text>{{ correoUsuario }}</b-card-text>
                        </b-card>
                    </b-col>
                </b-row>
            </b-col>
            <b-col col md="8">
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
                    <b-col>
                        <b-form-group
                        id="input-group-ubicacion"
                        label="Ubicacion"
                        label-for="input-ubicacion"
                        >
                            <b-form-input
                            id="input-ubicacion"
                            v-model="form.ubicacion"
                            type="url"
                            placeholder=""
                            ></b-form-input>
                        </b-form-group>

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
                    label-for="input-comentario"
                    >
                        <b-form-input 
                            id="input-comentario" 
                            v-model="comentario" 
                            placeholder="Escribir un comentario"
                        ></b-form-input>
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
            show: true,
            comentario:''
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
                console.log(response.data.data.den.denTitulo);
                this.form.titulo = response.data.data.den.denTitulo
                this.form.descripcion = response.data.data.den.denDescripcion
                const fecha = new Date(response.data.data.den.denFecha);
                this.form.fecha = fecha;
                this.form.ubicacion = "google.maps.com";

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
                    
                    this.form.tipo = tipoSeleccionado;
                }
                this.imgDen1 = response.data.data.denImagenes[0];
                this.imgDen2 = response.data.data.denImagenes[1];
                this.correoUsuario = response.data.data.den.denUsu;
            })
            .catch(error => {
                console.error(error);
            });
        }
      }
    }
  </script>