<template>
    <b-container class="p-3 m-auto">
        <b-row>
            <b-col col md="9" >
                <h2>Administrar > Agregar usuario</h2>
            </b-col>

            <b-col col md="3" >
                
            </b-col>
        </b-row>
        <b-row>
            <b-col col md="4">
                <b-card
                    border-variant="secondary"
                    header="Foto de perfil"
                    header-border-variant="secondary"
                    align="center"
                >
                    <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=58" rounded="circle" alt="Image 2"></b-img>
                    <b-card-text></b-card-text>
                </b-card>
            </b-col>
            <b-col col md="8">
                <b-form @submit="onSubmit" v-if="show">
                  
                 <b-row>
                    <b-col>
                        <b-form-group
                        id="input-group-nombre"
                        label="Nombre"
                        label-for="input-nombre"
                        >
                            <b-form-input
                            id="input-nombre"
                            v-model="form.nombre"
                            type="text"
                            placeholder=""
                            ></b-form-input>
                        </b-form-group>

                        <b-form-group
                        id="input-group-ci"
                        label="Carnet de identidad"
                        label-for="input-ci"
                        >
                            <b-form-input
                            id="input-ci"
                            v-model="form.ci"
                            type="text"
                            placeholder=""
                            ></b-form-input>
                        </b-form-group>

                        <label for="date-nacimiento">Fecha Nacimiento:</label>
                        <b-form-datepicker id="date-nacimiento" v-model="form.fechaNacimiento" class="mb-2"></b-form-datepicker>
                    </b-col>
                    <b-col>
                        <b-form-group
                        id="input-group-apellidos"
                        label="Apellidos"
                        label-for="input-apellidos"
                        >
                            <b-form-input
                            id="input-apellidos"
                            v-model="form.apellidos"
                            type="text"
                            placeholder=""
                            ></b-form-input>
                        </b-form-group>

                        <b-form-group
                        id="input-group-celular"
                        label="Celular"
                        label-for="input-celular"
                        >
                            <b-form-input
                            id="input-celular"
                            v-model="form.celular"
                            type="number"
                            placeholder=""
                            ></b-form-input>
                        </b-form-group>

                        <b-form-group id="input-group-area" label="Area" label-for="input-area">
                            <b-form-select
                            id="input-area"
                            v-model="form.area"
                            :options="areas"
                            ></b-form-select>
                        </b-form-group>
                        
                    </b-col>
                 </b-row>   
                 <b-row class="p-2">
                    <b-form-group
                    id="input-group-correo"
                    label="Correo electronico"
                    label-for="input-correo"
                    >
                        <b-form-input 
                            id="input-correo" 
                            v-model="form.email" 
                            type="text"
                            placeholder=""
                        ></b-form-input>
                    </b-form-group>
                    
                 </b-row>
                 <b-row class="p-3 bg-danger">
                    <b-col class="">
                        <b-button type="submit" variant="primary">Submit</b-button>
                        <b-button class="ml-2" @click="resetForm()">Reset</b-button>
                    </b-col>
                   
                 </b-row>
                   
                </b-form>
            </b-col>
        </b-row>
    </b-container>
   
  </template>
  
  <script>
    export default {
      data() {
        return { 
            form: {
            email: '',
            name: '',
            area: null
            },
            areas: [],
            show: true,
        }
      },
      computed: {
        
      },
      methods:{
        onSubmit(event){
            console.log('eviando')
        }
      }
    }
  </script>
  