<template>
  <div id="app">
    <EndOfContracts />
  </div>
</template>

<script>
import EndOfContracts from './views/EndOfContracts.vue'
export default {
  name: 'App',
  components: {
    EndOfContracts
  }
}
</script>

<style scoped>
#app {
  background-color: #000;
}
</style>
