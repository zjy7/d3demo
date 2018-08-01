<template>
  <div id="app">
    <Ech />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Ech from './components/ech.vue'
export default {
  name: 'app',
  components: {
    HelloWorld,Ech
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
