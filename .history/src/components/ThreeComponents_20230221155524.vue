<template>
  <div>
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import sun from '../assets/solar/太阳/8k_sun.jpg'
import mercury from '../assets/solar/水星/8k_mercury.jpg';
import venus from '../assets/solar/金星/8k_venus_surface.jpg'
import earth from '../assets/solar/地球/8k_earth_daymap.jpg'
import mars from '../assets/solar/火星/8k_mars.jpg'
import jupiter from '../assets/solar/木星/8k_jupiter.jpg'
import saturn from '../assets/solar/土星/8k_saturn.jpg'
import uranus from '../assets/solar/天王星/2k_uranus.jpg'
import neptune from '../assets/solar/海王星/2k_neptune.jpg'
export default {
  components: {
  },
  data() {
    return {
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      this.createScene() 
      this.createSunMesh() 
      this.createMercuryMesh()
      this.createVenusMesh()
      this.createEarthMesh()
      this.createMarsMesh()
      this.createJupiterMesh()
      this.createSaturnMesh()
      this.createUranusMesh()
      this.createNeptuneMesh()
      this.createLight() 
      this.createCamera() 
      this.createRender() 
      this.createControls() 
      this.creatAxesHelper()
      this.render() 

    },
    // 创建场景
    createScene() {
      this.scene = new THREE.Scene()
    },
    createSunMesh() {
      const geometry = new THREE.SphereGeometry(70, 200, 200) 
      const texture = new THREE.TextureLoader().load(sun)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(0, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createMercuryMesh() {
      const geometry = new THREE.SphereGeometry(15, 200, 200) 
      const texture = new THREE.TextureLoader().load(mercury)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(120, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createVenusMesh() {
      const geometry = new THREE.SphereGeometry(37.5, 200, 200) 
      const texture = new THREE.TextureLoader().load(venus)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(150, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createEarthMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(earth)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(400, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createMarsMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(mars)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(450, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createJupiterMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(jupiter)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(500, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createSaturnMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(saturn)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(600, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createUranusMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(uranus)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(700, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },
    createNeptuneMesh() {
      const geometry = new THREE.SphereGeometry(22, 200, 200) 
      const texture = new THREE.TextureLoader().load(neptune)
      const planeMaterial = new THREE.MeshLambertMaterial({
        map:texture,
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true
      // 设置位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(800, 0, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },

    // 创建光源
    createLight() {
      // 环境光
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // 创建环境光
      const spotLight = new THREE.SpotLight(0x888888) // 创建聚光灯
      spotLight.position.set(1500, 1200, 900)
      this.scene.add(ambientLight) // 将环境光添加到场景
      this.scene.add(spotLight)
    },
    // 创建相机
    createCamera() {
      const element = document.getElementById('container')
      const width = element.clientWidth // 窗口宽度
      const height = element.clientHeight // 窗口高度
      const k = width / height // 窗口宽高比
      this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 20000)
      this.camera.position.set(500, 2000, 3000) // 设置相机位置
      this.camera.lookAt(new THREE.Vector3(100, 0, 0)) // 设置相机方向
      this.scene.add(this.camera)
    },
    // 创建渲染器
    createRender() {
      const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
      this.renderer.shadowMap.enabled = false // 显示阴影
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色
      element.appendChild(this.renderer.domElement)
    },
    creatAxesHelper() {
      const axesHelper = new THREE.AxesHelper(250);
      axesHelper.setColors("pink", "red", "blue")
      this.scene.add(axesHelper);
    },
    render() { //渲染永远在最后执行
      this.renderer.render(this.scene, this.camera)
      requestAnimationFrame(this.render)
    },
    // 创建控件对象
    createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
    }
  }
}
</script>

<style>
#container {
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>

