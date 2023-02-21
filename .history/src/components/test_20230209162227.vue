<template>
  
  <div>
    <a-slider :value="value" :tooltip-visible="true" />
    <span>11111111111</span>
  </div>
</template>
<script>
import { defineComponent, ref } from 'vue';
export default defineComponent({
  setup() {
    const value = ref(30);
    return {
      value,
    };
  },
});
</script>