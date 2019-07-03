<template>
  <div id="app">
    <button @click="getJwt()">Obtener jwt</button>
    <div v-if="jwt">jwt actual: {{ jwt }}</div>
    <div v-else>Todavía no tengo ningún jwt</div>
    <div v-if="jwt">
      <button @click="sendMail()">
        Enviar correo
      </button>
    </div> 
    <div v-if="correoEnviado">Correo Enviado</div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data() {
    return {
      jwt: null,
      correoEnviado: false
    }
  },
  methods: {
    async getJwt() {
      const url = 'http://localhost:8000/api/get_jwt'
      let response = await axios.get(url)
      this.jwt = response.data.jwt
    },
    async sendMail() {
      const url = 'http://localhost:8000/api/calce_contact'
      let data = {
        customer_name: "juan perez",
        customer_email: "email@example.com",
        message: "Just a simple message"
      }
      const headers = {
        HTTP_AUTHORIZATION: this.jwt
      }
      let response = await axios.post(url, data, {headers})
      if(response.status !== 200) {
        this.correoEnviado = false
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
