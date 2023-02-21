<template>
  <div>
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
// import airplane from "../assets/glb/747airplane"
// import background from "../assets/img/background.png"
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
    // 初始化
    init() {
      this.createScene() // 创建场景
      this.createMesh() // 创建网格模型
      this.createLight() // 创建光源
      this.createCamera() // 创建相机
      this.createRender() // 创建渲染器
      this.createControls() // 创建控件器
      this.creatAxesHelper() // xyz轴承线 
      this.creatAnimation() // 动画
      this.creatEnableAnimation() //动画关联器
      this.update()//
      this.render() // 渲染

    },
    // 创建场景
    createScene() {
      this.scene = new THREE.Scene()
    },
    // 创建网格模型
    createMesh() {
      const geometry = new THREE.SphereGeometry(14, 30, 30) // 创建一个平面对象PlaneGeometry
      const planeMaterial = new THREE.MeshLambertMaterial({
        color: 0x777777
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true

      // 设置平面位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(0, -50, 0)
      // plane.
      // 平面对象添加到场景中
      this.scene.add(plane)
    },

    // 创建光源
    createLight() {
      // 环境光
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // 创建环境光
      this.scene.add(ambientLight) // 将环境光添加到场景

      const spotLight = new THREE.SpotLight(0x888888) // 创建聚光灯
      spotLight.position.set(10, 100, 1000)
      this.scene.add(spotLight)
    },
    // 创建相机
    createCamera() {
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
    updateMesh(selectMesh) {
      this.scene.remove(this.activeMesh)
      switch (selectMesh) {
        case 'cube':
          this.activeMesh = this.cube
          break
        case 'sphere':
          this.activeMesh = this.sphere
          break
        case 'plane':
          this.activeMesh = this.plane
          break
      }
      this.scene.add(this.activeMesh)
    },
    updateSide(side) {
      switch (side) {
        case 'front':
          this.meshMaterial.side = THREE.FrontSide
          break
        case 'back':
          this.meshMaterial.side = THREE.BackSide
          break
        case 'double':
          this.meshMaterial.side = THREE.DoubleSide
          break
      }
      this.meshMaterial.needsUpdate = true
    },
    // 更新属性
    updateFun() {
      this.meshMaterial.opacity = this.properties.opacity.value
      this.meshMaterial.transparent = this.properties.transparent
      this.meshMaterial.emissive.setStyle(this.properties.color)
      this.meshMaterial.visible = this.properties.visible

      this.updateMesh(this.properties.selectMesh)
      this.updateSide(this.properties.side)
      this.activeMesh.rotation.y += 0.01 //旋转
      this.atmosphere.rotation.y -= 0.02
      // console.log(this.delta);
      // console.log(this.mixer);
    },
    update() {
      var delta = this.clock.getDelta()
      this.mixer.update(delta)
    },
    render() { //渲染永远在最后执行
      this.update()
      this.updateFun()
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

.controls-box {
  position: absolute;
  right: 5px;
  top: 5px;
  width: 300px;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #c3c3c3;
}

.label-col {
  padding: 8px 5px;
}

.color-select-layer {
  position: relative;
  left: -20px;
  padding: 15px 0;
}

.vertice-span {
  line-height: 38px;
  padding: 0 2px 0 10px;
}
</style>

