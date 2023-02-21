<template>
  <div>
    <div id="solar"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import img from "../assets/img/earth.jpg"
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
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
      this.creatScene()
      this.createCamera()
      this.createRender()
      this.createMesh()
      this.createControls()
      this.render()
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
      Sun.name="Sun"
      this.scene.add(Sun)
    },
    render(){
      this.renderer.render(this.scene,this.camera)
      requestAnimationFrame(this.render)
    },
    createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
    }
  },
};
</script>

<style>

</style>