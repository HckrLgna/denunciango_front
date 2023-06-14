<template>
    <div>
        <b-container>
          <b-row class="py-3">
            <h3>Mapa </h3>
          </b-row>
          <b-row>
            <b-form @submit="onSubmit" @reset="onReset" v-if="show">
                <b-form-row>
                    <b-form-group id="input-group-3" label="Tipo de denuncia:" label-for="input-3">
                        <b-form-select
                        id="input-3"
                        v-model="form.food"
                        :options="foods"
                        required
                        ></b-form-select>
                    </b-form-group>
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
        foods: [{ text: 'Basura', value: null }, 'Ruido', 'Alumbrado', 'Carretera', 'Corn'],
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