<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <ThreeComponents msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import ThreeComponents from '@/components/ThreeComponents.vue'

export default {
  name: 'Home',
  components: {
    ThreeComponents
  }
}
</script>
