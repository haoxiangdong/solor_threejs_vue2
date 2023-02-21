<template>
    <div>
        <div id="container"></div>
    </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
// import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
import img from "../assets/img/earth.jpg"
export default {
    name: 'ModelImport20230213',

    data() {
        return {
            scene: null,
            camera: null,
            renderer: null,
            controls:null,

        };
    },

    mounted() {
        this.init()
    },

    methods: {
        //初始化
        init() {
            this.creatScene()
            this.createCamera()
            this.createMesh()
            this.createLight()
            this.createControls()
            this.creatAxesHelper()
            this.createRender()
            this.render()
        },
        //创建3D场景
        creatScene() {
            this.scene = new THREE.Scene()
        },
        //创建相机
        createCamera() {
            const element = document.getElementById('container')
            const width = element.clientWidth // 窗口宽度
            const height = element.clientHeight // 窗口高度
            const k = width / height // 窗口宽高比
            this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 1000)
            this.camera.position.set(300, 300, 500) // 设置相机位置
            this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // 设置相机方向
            this.scene.add(this.camera)
        },
        //创建网格模型
        createMesh() {
      const geometry = new THREE.PlaneGeometry(200, 200, 10, 10) // 创建一个平面对象PlaneGeometry
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
      // this.scene.add(plane)
      const sphereGeometry = new THREE.SphereGeometry(14, 30, 30)
      const atmosphereGeometry = new THREE.SphereGeometry(20, 30, 30)
      const cubeGeometry = new THREE.BoxGeometry(15, 15, 15)
      const planeGeometry = new THREE.PlaneGeometry(14, 14, 4, 4)

      //从外部读取贴图
      var texture = new THREE.TextureLoader().load(img)
      var atmosphereTexture = new THREE.TextureLoader().load(img)
      // 创建材质
      this.meshMaterial = new THREE.MeshPhongMaterial({
        map: texture
      })
      // 创建大气材质
      const atmosphereMeshMaterial = new THREE.MeshPhongMaterial({
        map: atmosphereTexture,
        color: '#1b95f0'
      })
      atmosphereMeshMaterial.transparent = true
    //   atmosphereMeshMaterial.opacity = this.properties.opacity.value



      // 创建球、方块、平面
      this.sphere = new THREE.Mesh(sphereGeometry, this.meshMaterial)
      this.atmosphere = new THREE.Mesh(atmosphereGeometry, atmosphereMeshMaterial)
      this.cube = new THREE.Mesh(cubeGeometry, this.meshMaterial)
      this.plane = new THREE.Mesh(planeGeometry, this.meshMaterial)

    //   this.sphere.position.set(50, -5, 0)
    //   this.cube.position = this.sphere.position
    //   this.plane.position = this.sphere.position
    //   this.activeMesh = this.sphere
      this.scene.add(this.atmosphere)
      // })

    },
        //创建光源
        createLight() {
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // 创建环境光
            this.scene.add(ambientLight) // 将环境光添加到场景
            const spotLight = new THREE.SpotLight(0x888888) // 创建聚光灯
            spotLight.position.set(10, 100, 1000)
            this.scene.add(spotLight)
        },
        //创建控制器
        createControls() {
            this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
        },
        //创建xyz辅助线
        creatAxesHelper() {
            const axesHelper = new THREE.AxesHelper(250);
            axesHelper.setColors("pink", "red", "blue")
            this.scene.add(axesHelper);
        },
        //创建渲染器
        createRender() {
            const element = document.getElementById('container')
            this.renderer = new THREE.WebGLRenderer()
            this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
            this.renderer.shadowMap.enabled = true // 显示阴影
            this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色
            this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
            element.appendChild(this.renderer.domElement)
        },
        loadGlbModel() {

        },
        render() { //渲染永远在最后执行
            this.renderer.render(this.scene, this.camera)
            requestAnimationFrame(this.render)
        },
    },
};
</script>

<style>
#container {
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>