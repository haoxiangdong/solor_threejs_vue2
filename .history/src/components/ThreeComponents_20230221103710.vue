<template>
  <div>
    <div id="container">
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
// import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
export default {
  name: 'ModelImportThreeComponents',

  data() {
    return {
      scene:null,
    };
  },

  mounted() {
    this.init()
  },

  methods: {
    init(){},
    createScene(){
      this.scene = new THREE.Scene()
    },
    createMesh(){
      const SunGeometry = new THREE.ShapeBufferGeometry(200, 200, 10, 10) 
  },
};
</script>

<style lang="scss" scoped>

</style>