<template>
  <div>
    <div id="container">
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

export default {
  name: 'ModelImportThreeComponents',

  data() {
    return {
      
    };
  },

  mounted() {
    this.init();
  },

  methods: {
    init(){
      this.createScene() // 创建场景
      this.createMesh() // 创建网格模型
      this.createLight() // 创建光源
      this.createCamera() // 创建相机
      this.createRender() // 创建渲染器
      this.createControls() // 创建控件器
      this.creatAxesHelper() // xyz轴承线 
      // this.creatAnimation() // 动画
      // this.creatEnableAnimation() //动画关联器
      this.update()//
      this.render() // 渲染
    },
    createScene(){
      this.scene = new THREE.Scene()
    },
    createMesh(){
      const geometry = new THREE.PlaneGeometry(200, 200, 10, 10) // 创建一个平面对象PlaneGeometry
      const planeMaterial = new THREE.MeshLambertMaterial({
        color: 0x777777
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true

      // 设置平面位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(0, -50, 0)
      this.scene.add(plane)
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
    createRender(){
      const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
      this.renderer.shadowMap.enabled = false // 显示阴影
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色
      element.appendChild(this.renderer.domElement)
    },

    creatAxesHelper(){},
    update(){},
    render(){
      this.renderer.render(this.scene, this.camera)
      requestAnimationFrame(this.render)
    },
    createControls(){
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
    },
  },
};
</script>

<style lang="scss" scoped>

</style>