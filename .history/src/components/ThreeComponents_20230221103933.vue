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
      const SunGeometry = new THREE.SphereGeometry(200, 200, 10, 10) 
      const SunMaterial = new THREE.MeshBasicMaterial({
        color:0xeb1407
      })
      const Sun =new THREE.Mesh(SunGeometry,SunMaterial)
      this.scene.add(Sun)
  },
};
</script>

<style lang="scss" scoped>

</style>