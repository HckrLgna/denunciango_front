<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="9" >
                <h2>Administrar Area > Agregar area</h2>
            </b-col>
          
        </b-row>
        <b-row>
            <b-col col md="5" class="p-3">
                <div>
                    <b-img src="https://picsum.photos/1024/400/?image=41" fluid alt="Responsive image"></b-img>
                </div>
            </b-col>
            <b-col col md="7" class="p-3">
                <b-form @submit="onSubmit" >
                 <b-row>
                    <b-form-group
                        id="input-group-nombre"
                        label="Nombre del area"
                        label-for="input-nombre"
                        >
                        <b-form-input
                            id="input-nombre"
                            v-model="form.nombre"
                            type="text"
                            placeholder="Ingrese el nombre del area"
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
                    <b-form-group
                        id="input-group-nombre"
                        label="Nombre del area"
                        label-for="input-nombre"
                        >
                            <b-form-input
                                id="input-celular"
                                v-model="form.celular"
                                type="number"
                                placeholder="Ingrese el numero de celular del area"
                                required
                            ></b-form-input>
                    </b-form-group>
                    <b-form-group
                        id="input-group-direccion"
                        label="Direccion"
                        label-for="input-direccion"
                        >
                        <b-form-input
                            id="input-direccion"
                            v-model="form.direccion"
                            type="text"
                            placeholder="Ingrese la direccion del area"
                            required
                        ></b-form-input>
                    </b-form-group>

                        <b-form-group class="p-3">
                            <b-button type="submit" variant="primary">Registrar</b-button>
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
                nombre:'',
                descripcion:'',
                celular:'',
                direccion:''
            },
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
            axios.post('http://cuidomivoto.com/api/areRegistrarArea',)
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