<template>
  <div id="app">
    <Home></Home>
  </div>
</template>

<script>
import Home from './views/Home.vue'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>

<style>
#app{
  padding: 0;
  margin: 0;
}
body{
  padding: 0;
  margin: 0;
}
</style>
