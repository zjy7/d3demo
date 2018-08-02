<template>
  <div id="app">
    <Ech2 />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Ech from './components/ech.vue'
import Ech2 from './components/ech2.vue'
export default {
  name: 'app',
  components: {
    HelloWorld,Ech,Ech2
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
