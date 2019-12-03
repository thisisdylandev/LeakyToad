<template>
  <div id="app">
    <BrewerySearch/>
  </div>
</template>

<script>
import BrewerySearch from './components/BrewerySearch.vue'

export default {
  name: 'app',
  components: {
    BrewerySearch
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
