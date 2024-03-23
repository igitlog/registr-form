<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <RegistrForm msg="Welcome to Registr form"/>
  </div>
</template>

<script>
import RegistrForm from './components/RegistrForm.vue'

export default {
  name: 'App',
  components: {
    RegistrForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
