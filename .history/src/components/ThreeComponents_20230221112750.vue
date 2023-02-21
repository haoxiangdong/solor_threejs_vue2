<template>
  <div>
    <div id="container">
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
export default {
  name: 'ModelImportThreeComponents',

  data() {
    return {
      scene:null,
      camera:null,
      renderer:null,
      controls:null,

    };
  },

  mounted() {
    this.init()
  },

  methods: {
    init(){
      this.createScene()
      this.createMesh()
      this.createLight()
      this.createCamera()
      this.creatRender()
      this.creatAxesHelper()
      this.render()
    },
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
  createLight(){
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // 创建环境光
      this.scene.add(ambientLight) // 将环境光添加到场景
      const spotLight = new THREE.SpotLight(0x888888) // 创建聚光灯
      spotLight.position.set(10, 100, 1000)
      this.scene.add(spotLight)
  },
  createCamera(){
    const element = document.getElementById('container')
      const width = element.clientWidth // 窗口宽度
      const height = element.clientHeight // 窗口高度
      const k = width / height // 窗口宽高比
      // PerspectiveCamera( fov, aspect, near, far )
      this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 1000)
      this.camera.position.set(300, 300, 500) // 设置相机位置

      this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // 设置相机方向
      this.scene.add(this.camera)
  },
  creatAxesHelper() {
      const axesHelper = new THREE.AxesHelper(250);
      axesHelper.setColors("pink", "red", "blue")
      this.scene.add(axesHelper);
    },
  creatRender(){
    const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
      this.renderer.shadowMap.enabled = false // 显示阴影
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色
      element.appendChild(this.renderer.domElement)
  },
  render(){
    this.renderer.render(this.scene, this.camera)
    requestAnimationFrame(this.render)
  },
  createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
    }
}
}
</script>

<style lang="scss" scoped>

</style>