<template>
  <div class="login">
    
    <div class="mt-5 login-content">
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-1"
          label="Usuario"
          label-for="input-1"
          description="Correo electrónico."
        >
          <b-form-input
            id="input-1"
            v-model="form.email"
            type="email"
            placeholder=""
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Contraseña:" label-for="input-2">
          <b-form-input
            id="input-2"
            v-model="form.name"
            type="password"
            required
          ></b-form-input>
        </b-form-group>        

        <b-form-group id="input-group-4" v-slot="{ ariaDescribedby }">
          <b-form-checkbox-group
            v-model="form.checked"
            id="checkboxes-4"
            :aria-describedby="ariaDescribedby"
          >
            <b-form-checkbox value="me">Recordar datos</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>

        <b-button type="submit" variant="primary" class="mr-2">Ingresar</b-button>
        <b-button type="reset" variant="danger">Volver</b-button>
      </b-form>      
    </div>
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
        foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
        show: true
      }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        alert(JSON.stringify(this.form))
        this.$router.push('admin')
      },
      onReset(event) {
        event.preventDefault()
        this.$router.push('/')
      }
    }
  }
</script>
<style>
  .login {
    display: flex;
    justify-content: center;
    align-content: center;
  }
  .login .login-content{
    background-color: skyblue;
    padding: 15px;
    max-width: 400px;
    border-radius: 10px;
  }
</style>