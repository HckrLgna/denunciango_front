<template>
    <div>
        <b-container>
          <b-row class="py-3">
            <h3>Mapa </h3>
          </b-row>
          <b-row>
            <b-form @submit="onSubmit" @reset="onReset" v-if="show">
                <b-form-row>
                    <b-form-group id="input-group-3" label="Tipo de denuncia:" label-for="input-tipo">
                        <b-form-select
                        id="input-tipo"
                        v-model="form.tipo"
                        :options="tipos"
                        required
                        ></b-form-select>
                    </b-form-group>
                </b-form-row>
                <b-form-row>
                    <b-form-group id="input-group-3" label="Estado de denuncia:" label-for="input-estado">
                        <b-form-select
                        id="input-estado"
                        v-model="form.estado"
                        :options="estados"
                        required
                        ></b-form-select>
                    </b-form-group>
                </b-form-row>
                <b-form-row>
                  <div class="mb-3">
                    <b-form-datepicker
                      v-model="form.fecha_inicio"
                      placeholder="Fecha de inicio"
                    ></b-form-datepicker>
                    <b-form-datepicker
                      v-model="form"
                      placeholder="Fecha de fin"
                    ></b-form-datepicker>
                  </div>
                </b-form-row>
            </b-form>
          </b-row>
            
        </b-container>
    </div>
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
        estados: [],
        tipos: [],
        show: true
      }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        alert(JSON.stringify(this.form))
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>