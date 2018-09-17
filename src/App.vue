<template>
  <div id="app">
    <MatrixMines/>
  </div>
</template>

<script>
import MatrixMines from './components/MatrixMines.vue'

export default {
  name: 'app',
  components: {
    MatrixMines
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
