<template>
  <img alt="Vue logo" src="./assets/logo.png" height="50px">
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <CaseStudy />
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import CaseStudy from './components/CaseStudy.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    CaseStudy
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
