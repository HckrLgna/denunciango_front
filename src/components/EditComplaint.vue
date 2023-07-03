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
                        <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=54" alt="Image 1"></b-img>
                    </b-col>
                    <b-col>
                        <b-img thumbnail fluid src="https://picsum.photos/250/250/?image=58" alt="Image 2"></b-img>
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
                            <b-card-text>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</b-card-text>
                        </b-card>
                    </b-col>
                </b-row>
            </b-col>
            <b-col col md="8">
                <b-form @submit="onSubmit"  v-if="show">
                 <b-row>
                    <b-form-group
                    id="input-group-1"
                    label="Titulo"
                    label-for="input-1"
                    >
                    <b-form-input
                    id="input-1"
                    v-model="form.email"
                    type="email"
                    placeholder=""
                    required
                    ></b-form-input>
                    </b-form-group>

                    <b-form-group id="input-group-2" label="Descripcion" label-for="textarea-descripcion">
                        <b-form-textarea
                            id="textarea-descripcion"
                            placeholder="Fixed height textarea"
                            rows="3"
                            no-resize
                        ></b-form-textarea>
                    </b-form-group>
                 </b-row>
                 <b-row>
                    <b-col>
                        <label for="example-datepicker">Fecha:</label>
                        <b-form-datepicker id="example-datepicker" v-model="value" class="mb-2"></b-form-datepicker>
                        <b-form-group id="input-group-3" label="Food:" label-for="input-3">
                            <b-form-select
                            id="input-3"
                            v-model="form.food"
                            :options="foods"
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
                            type="name"
                            placeholder="google.com"
                            ></b-form-input>
                        </b-form-group>
                        <b-form-group id="input-group-3" label="Food:" label-for="input-3">
                            <b-form-select
                            id="input-3"
                            v-model="form.food"
                            :options="foods"
                            required
                            ></b-form-select>
                        </b-form-group>
                        
                    </b-col>
                 </b-row>   
                 <b-row>
                    <b-form-group
                    id="input-group-4"
                    label="Comentario de respuesta"
                    label-for="input-comentario"
                    >
                        <b-form-input id="input-comentario" v-model="comentario" placeholder="Escribir un comentario"></b-form-input>
                    </b-form-group>
                    
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
            food: null,
            checked: []
            },
            foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
            show: true,
            comentario:''
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