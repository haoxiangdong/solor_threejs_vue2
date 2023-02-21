<template>
  <div>
    <div id="container"></div>
</div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import sun_img from '../assets/solar/太阳/8k_sun.jpg'
import mercury_img from '../assets/solar/水星/8k_mercury.jpg';
import venus_img from '../assets/solar/金星/8k_venus_surface.jpg'
import earth_img from '../assets/solar/地球/8k_earth_daymap.jpg'
import mars_img from '../assets/solar/火星/8k_mars.jpg'
import jupiter_img from '../assets/solar/木星/8k_jupiter.jpg'
import saturn_img from '../assets/solar/土星/8k_saturn.jpg'
import uranus_img from '../assets/solar/天王星/2k_uranus.jpg'
import neptune_img from '../assets/solar/海王星/2k_neptune.jpg'
export default {
  components: {
  },
  data() {
    return {
      scene:null,
      camera:null,
      renderer:null,
      sun:null,
      mercury:null,
      venus:null,
      earth:null,
      mars:null,
      jupiter:null,
      saturn:null,
      uranus:null,
      neptune:null,
      solarGroup:null,
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
      // this.createSolarGroup()
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
      const geometry = new THREE.SphereGeometry(120, 20, 20)
      const texture = new THREE.TextureLoader().load(sun_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.sun = new THREE.Mesh(geometry, material)
      this.sun.receiveShadow = true
      // 设置位置
      // plane.rotation.x = -0.5 * Math.PI
      this.sun.position.set(0, 0, 0)
      // plane.
      this.scene.add(this.sun)
    },
    createMercuryMesh() {
      const geometry = new THREE.SphereGeometry(15, 20, 20)
      const texture = new THREE.TextureLoader().load(mercury_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.mercury = new THREE.Mesh(geometry, material)
      this.mercury.receiveShadow = true
      // 设置位置
      this.mercury.position.set(170, 0, 0)
      // plane.
      this.scene.add(this.mercury)
    },
    createVenusMesh() {
      const geometry = new THREE.SphereGeometry(24, 20, 20)
      const texture = new THREE.TextureLoader().load(venus_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.venus = new THREE.Mesh(geometry, material)
      this.venus.receiveShadow = true
      // 设置位置
      this.venus.position.set(280, 0, 0)
      // venus.
      this.scene.add(this.venus)
    },
    createEarthMesh() {
      const geometry = new THREE.SphereGeometry(27, 20, 20)
      const texture = new THREE.TextureLoader().load(earth_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.earth = new THREE.Mesh(geometry, material)
      this.earth.receiveShadow = true
      // 设置位置
      this.earth.position.set(400, 0, 0)
      // earth.
      this.scene.add(this.earth)
    },
    createMarsMesh() {
      const geometry = new THREE.SphereGeometry(25, 20, 20)
      const texture = new THREE.TextureLoader().load(mars_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.mars = new THREE.Mesh(geometry, material)
      this.mars.receiveShadow = true
      // 设置位置
      this.mars.position.set(520, 0, 0)
      // mars.
      this.scene.add(this.mars)
    },
    createJupiterMesh() {
      const geometry = new THREE.SphereGeometry(57, 20, 20)
      const texture = new THREE.TextureLoader().load(jupiter_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.jupiter = new THREE.Mesh(geometry, material)
      this.jupiter.receiveShadow = true
      // 设置位置
      this.jupiter.position.set(700, 0, 0)
      // jupiter.
      this.scene.add(this.jupiter)
    },
    createSaturnMesh() {
      const geometry = new THREE.SphereGeometry(45, 200, 200)
      const texture = new THREE.TextureLoader().load(saturn_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.saturn = new THREE.Mesh(geometry, material)
      this.saturn.receiveShadow = true
      // 设置位置
      this.saturn.position.set(850, 0, 0)
      // saturn.
      this.scene.add(this.saturn)
    },
    createUranusMesh() {
      const geometry = new THREE.SphereGeometry(34, 20, 20)
      const texture = new THREE.TextureLoader().load(uranus_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.uranus = new THREE.Mesh(geometry, material)
      this.uranus.receiveShadow = true
      // 设置位置
      this.uranus.position.set(950, 0, 0)
      // uranus.
      this.scene.add(this.uranus)
    },
    createNeptuneMesh() {
      const geometry = new THREE.SphereGeometry(33, 20, 20)
      const texture = new THREE.TextureLoader().load(neptune_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
      }) // 材质对象Material
      this.neptune = new THREE.Mesh(geometry, material)
      this.neptune.receiveShadow = true
      // 设置位置
      this.neptune.position.set(1050, 0, 0)
      // neptune.
      this.scene.add(this.neptune)
    },//创建组
    createSolarGroup(){
      this.solarGroup=new THREE.Group();
      this.solarGroup.add(this.sun)
      this.solarGroup.add(this.mercury)
      this.solarGroup.add(this.venus)
      this.solarGroup.add(this.earth)
      this.solarGroup.add(this.mars)
      this.solarGroup.add(this.jupiter)
      this.solarGroup.add(this.saturn)
      this.solarGroup.add(this.uranus)
      this.solarGroup.add(this.neptune)
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
    update(){
      this.sun.rotation.y+=0.01
      this.sun.rotation.x=1/10*Math.PI
      this.mercury.rotation.y+=0.02
      this.earth.rotation.y+=0.005
      this.earth.rotation.x=5/38*Math.PI
      this.mars.rotation.y+=0.001
      this.mars.rotation.x=1/7.14*Math.PI
      this.jupiter.rotation.y+=0.005
      this.jupiter.rotation.x=1/60*Math.PI
      this.saturn.rotation.y+=0.005
      this.jupiter.rotation.x=10/67*Math.PI
      this.neptune.rotation.y+=0.001
      this.neptune.rotation.x=10/63*Math.PI
      this.venus.rotation.y-=0.1
      this.venus.rotation.x=1/66*Math.PI
      this.uranus.rotation.y-=0.007
      this.uranus.rotation.x=-0.5*Math.PI
      // this.solarGroup.rotation.y+=0.001
    },
    render() { //渲染永远在最后执行
      this.update()
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

