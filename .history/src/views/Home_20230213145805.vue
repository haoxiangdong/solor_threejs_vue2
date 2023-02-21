<template>
  <div class="home">
    <div>
      <router-link to="/about">About</router-link>
      <router-view></router-view>
    </div>
    <ThreeComponent/>
  </div>
</template>

<script>
import ThreeComponent from '@/components/ThreeComponent.vue'

export default {
  name: 'Home',
  components: {
    ThreeComponent
  }
}
</script>
<style>
</style>
