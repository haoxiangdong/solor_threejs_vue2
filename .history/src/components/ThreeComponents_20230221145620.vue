<template>
  <div>
    <div id="solar"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import img from "../assets/img/earth.jpg"
export default {
  name: 'ModelImportThreeComponents',

  data() {
    return {
      scene:null,
      camera:null,
      renderer:null,

    };
  },

  mounted() {
    this.init()
  },

  methods: {
    init(){

    },
    creatScene(){
      this.scene=new THREE.scene()
    },
    createCamera(){
      const width = window.clientWidth 
      const height = window.clientHeight 
      const k = width / height 
      this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 1000)
      this.camera.position.set(300, 300, 500) 
      this.camera.lookAt(new THREE.Vector3(10, 0, 0)) 
      this.scene.add(this.camera)
    },
    createRender(){
      this.renderer = new THREE.WebGLRenderer({
        alpha: true,
        antialias: true
      })
      this.renderer.setSize(window.clientWidth, window.clientHeight) 
      this.renderer.shadowMap.enabled = true 
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0xffffff, 1)
      window.appendChild(this.renderer.domElement)
    },
    createMesh(){
      const SunGeometry = new THREE.SphereGeometry(14, 30, 30)
      var texture = new THREE.TextureLoader().load(img)
      const SunMaterial = new THREE.MeshPhongMaterial({
        map: texture
      })
      const Sun=new THREE.Mesh(SunGeometry,SunMaterial)
      this.scene.add(Sun)
    },
  },
};
</script>

<style>

</style>