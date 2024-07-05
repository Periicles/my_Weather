<template>
  <img alt="Vue logo" src="./assets/weather-app.png">
  <weatherApp msg="My Weather"/>
</template>

<script>
import weatherApp from './components/weather-app.vue'

export default {
  name: 'App',
  components: {
    weatherApp
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
  background-color: #6495ED;
  min-height: 100vh;
}

img {
  margin-top: 50px;
  max-width: 50%;
  height: auto;
}
</style>
