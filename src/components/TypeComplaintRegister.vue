<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="9" >
                <h2>Administrar Area > Agregar Tipo de denuncia</h2>
            </b-col>
          
        </b-row>
        <b-row>
            <b-col col md="5" class="p-3">
                <div>
                    <img src="../assets/img_type.png" class="w-100" fluid alt="Responsive image">
                </div>
            </b-col>
            <b-col col md="7" class="p-3">
                <form ref="form" @submit="onSubmit" >
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
                    <b-form-group class="p-3">
                        <b-button type="submit" variant="primary">Registrar</b-button>
                    </b-form-group>
                </form>
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
            
            nombreDenuncia: '',
            nameState: null,
            submittedNames: [],
            descripcionDenuncia:'',

        }
      },
      mounted() {
         

      },
      computed: {
        
      },
      methods:{
        
        onSubmit(event){
            console.log('eviando')
            event.preventDefault();
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
         
      }
    }
  </script>
<style>
 
</style>