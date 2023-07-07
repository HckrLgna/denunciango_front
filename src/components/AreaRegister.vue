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
                    <img src="../assets/img_area.png" class="w-100" fluid alt="Responsive image">
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
                        label="Celular del area"
                        label-for="input-nombre"
                        >
                            <b-form-input
                                id="input-celular"
                                v-model="form.celular"
                                type="text"
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
                    <b-form-group
                        label="Lista de tipos de denuncia"
                        v-slot="{ ariaDescribedby }"
                        >
                        <b-form-checkbox-group
                            v-model="tipo"
                            :options="tipos"
                            :aria-describedby="ariaDescribedby"
                            name="flavour-2a"
                            stacked
                        ></b-form-checkbox-group>
                        <b-link v-b-modal.modal-prevent-closing>Agregar Nuevo</b-link>
                        <b-modal
                            id="modal-prevent-closing"
                            ref="modal"
                            title="Nuevo tipo de denuncia"
                            @show="resetModal"
                            @hidden="resetModal"
                            @ok="handleOk"
                            >
                            <form ref="form" @submit.stop.prevent="handleSubmit">
                                <b-form-group
                                    label="Nombre del tipo de denuncia"
                                    label-for="nombre-input"
                                    invalid-feedback="El nombre es requerido"
                                    :state="nameState"
                                    >
                                    <b-form-input
                                        id="nombre-input"
                                        v-model="nombreDenuncia"
                                        :state="nameState"
                                        required
                                    ></b-form-input>
                                </b-form-group>
                                <b-form-group id="input-group-descripcion" label="Descripcion" label-for="descripcion-textarea">
                                    <b-form-textarea
                                        id="descripcion-textarea"
                                        placeholder="Ingrese descripcion del area"
                                        v-model="descripcionDenuncia"
                                        rows="3"
                                        no-resize
                                    ></b-form-textarea>
                                </b-form-group>
                            </form>
                        </b-modal>
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
                direccion:'',
                
            },
            tipo:[],
            tipos: [],
            nombreDenuncia: '',
            nameState: null,
            submittedNames: [],
            descripcionDenuncia:'',

        }
      },
      mounted() {
        axios.get('https://denunciangows.fly.dev/api/tiposDenuncia')
            .then(response => {
                console.log(response.data.data);
                this.tipos = response.data.data.map(dat => ({value: dat.tdId, text: dat.tdTitulo}) );
            })
            .catch(error => {
                console.error(error);
        });

      },
      computed: {
        
      },
      methods:{
        checkFormValidity() {
            const valid = this.$refs.form.checkValidity()
            this.nameState = valid
            return valid
        },
        resetModal() {
            this.nombreDenuncia = ''
            this.nameState = null
            this.descripcionDenuncia=''
        },
        handleOk(bvModalEvent) {
            // Prevent modal from closing
            bvModalEvent.preventDefault()
            // Trigger submit handler
            this.handleSubmit()
        },
        handleSubmit() {
            // Exit when the form isn't valid
            if (!this.checkFormValidity()) {
            return
            }
            // Push the name to submitted names
            this.submittedNames.push(this.nombreDenuncia)
            // Hide the modal manually
            this.$nextTick(() => {
            this.$bvModal.hide('modal-prevent-closing')
            this.onSubmitTipo();
            })
        },
        onSubmitTipo(event){
            console.log('eviando')
            const data = {
                    tdTitulo: this.nombreDenuncia,
                    tdDesc: this.descripcionDenuncia,   
            };
            console.log(data);
            axios
            .post('http://cuidomivoto.com/api/tdRegistrarTipoDen', data)
            .then(response => {
                console.log(response.data);
            })
            .catch(error => {
                console.error(error);
            });
                console.log(data);
        },
        onSubmit(event){
            console.log('eviando')
            event.preventDefault();
            const data = {
                area: {
                areNombre: this.form.nombre,
                areDesc: this.form.descripcion,
                areCelular: this.form.celular,
                areDireccion: this.form.direccion
                },
                tipos: this.tipo.map(item => ({ atTd: item }))
            };
            axios
            .post('http://cuidomivoto.com/api/areRegistrarArea', data)
            .then(response => {
                console.log(response.data);
            })
            .catch(error => {
                console.error(error);
            });
                console.log(data);
        },
         
      }
    }
  </script>
<style>
 
</style>