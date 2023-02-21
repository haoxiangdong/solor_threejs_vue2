<template>
  <div class="home">
    <ThreeComponent/>
  </div>
</template>

<script>
// @ is an alias to /src
import ThreeComponent from '@/components/ThreeComponent.vue'

export default {
  name: 'Home',
  components: {
    ThreeComponent
  }
}
</script>
