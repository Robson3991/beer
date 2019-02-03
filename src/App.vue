<template>
  <div id="app">
    <Beer />
  </div>
</template>

<script>

import Beer from './components/Beer.vue'

export default {
  name: 'app',
  components: {
    Beer
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 20px;
}
</style>
