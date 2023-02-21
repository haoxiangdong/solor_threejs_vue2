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
      scene: null,
      camera: null,
      renderer: null,
      sun: null,
      mercury: null,
      venus: null,
      earth: null,
      mars: null,
      jupiter: null,
      saturn: null,
      uranus: null,
      neptune: null,
      solarGroup: null,
      degree: 0,
      planet: [
        {
          name: "mercury",
          radios: 15,
          img: mercury_img,
          autorotationRate: 0.002,
          speed:360,
          x: 170,
          y: 0,
          z: 0,
        },
        {
          name: "venus",
          radios: 24,
          img: venus_img,
          autorotationRate: 0.01,
          speed:540,
          x: 280,
          y: 0,
          z: 0,
        },
        {
          name: "earth",
          radios: 27,
          img: earth_img,
          autorotationRate: 0.005,
          speed:720,
          x: 400,
          y: 0,
          z: 0,
        },
        {
          name: "mars",
          radios: 25,
          img: mars_img,
          autorotationRate: 0.001,
          speed:900,
          x: 520,
          y: 0,
          z: 0,
        },
        {
          name: "jupiter",
          radios: 57,
          img: jupiter_img,
          autorotationRate: 0.005,
          speed:1080,
          x: 700,
          y: 0,
          z: 0,
        },
        {
          name: "saturn",
          radios: 45,
          img: saturn_img,
          autorotationRate: 0.005,
          speed:1260,
          x: 850,
          y: 0,
          z: 0,
        },
        {
          name: "uranus",
          radios: 34,
          img: uranus_img,
          autorotationRate: 0.007,
          speed:1440,
          x: 950,
          y: 0,
          z: 0,
        },
        {
          name: "neptune",
          radios: 33,
          img: neptune_img,
          autorotationRate: 0.0001,
          speed:1620,
          x: 1050,
          y: 0,
          z: 0,
        },
      ]

    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      this.createScene()
      this.createSolarGroup()
      this.createSunMesh()
      this.creatPlanetMesh()
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
      // this.scene.background.dispose()

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
      this.solarGroup.add(this.sun)
    },
    creatPlanetMesh() {
      for (let index = 0; index < this.planet.length; index++) {
        const plane_index = this.planet[index]
        const geometry = new THREE.SphereGeometry(plane_index.radios, 20, 20)
        const texture = new THREE.TextureLoader().load(plane_index.img)
        const material = new THREE.MeshLambertMaterial({
          map: texture,
        })
        const planets = new THREE.Mesh(geometry, material)
        if (plane_index.name == "mercury") {
          this.mercury = planets
        } else if (plane_index.name == "venus") {
          this.venus = planets
        } else if (plane_index.name == "earth") {
          this.earth = planets
        } else if (plane_index.name == "mars") {
          this.mars = planets
        } else if (plane_index.name == "jupiter") {
          this.jupiter = planets
        } else if (plane_index.name == "saturn") {
          this.saturn = planets
        } else if (plane_index.name == "uranus") {
          this.uranus = planets
        } else if (plane_index.name == "neptune") {
          this.neptune = planets
        }
        planets.receiveShadow = true
        planets.position.set(plane_index.x, plane_index.y, plane_index.z)
        this.solarGroup.add(planets)
      }
    },
    createSolarGroup() {
      this.solarGroup = new THREE.Group();
      this.scene.add(this.solarGroup)
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
    update() {
      for (let index = 0; index < this.planet.length; index++) {
        const indexName = this.planet[index]
        const d = (++this.degree * Math.PI) / indexName.speed
        const x = Math.sin(d) * indexName.x
        const z = Math.cos(d) * indexName.x
        if (indexName.name == "mercury") {
          this.mercury.position.set(x, 0, z)
      this.mercury.rotation.y += indexName.autorotationRate
        } else if (indexName.name == "venus") {
          this.venus.position.set(x, 0, z)
        } else if (indexName.name == "earth") {
          this.earth.position.set(x, 0, z)
        } else if (indexName.name == "mars") {
          this.mars.position.set(x, 0, z)
        } else if (indexName.name == "jupiter") {
          this.jupiter.position.set(x, 0, z)
        } else if (indexName.name == "saturn") {
          this.saturn.position.set(x, 0, z)
        } else if (indexName.name == "uranus") {
          this.uranus.position.set(x, 0, z)
        } else if (indexName.name == "neptune") {
          this.neptune.position.set(x, 0, z)
        }
      }
      this.sun.rotation.y += 0.01
      this.sun.rotation.x = 1 / 10 * Math.PI
      this.earth.rotation.y += 0.005
      this.earth.rotation.x = 5 / 38 * Math.PI
      this.mars.rotation.y += 0.001
      this.mars.rotation.x = 1 / 7.14 * Math.PI
      this.jupiter.rotation.y += 0.005
      this.jupiter.rotation.x = 1 / 60 * Math.PI
      this.saturn.rotation.y += 0.005
      this.jupiter.rotation.x = 10 / 67 * Math.PI
      this.neptune.rotation.y += 0.001
      this.neptune.rotation.x = 10 / 63 * Math.PI
      this.venus.rotation.y -= 0.01
      this.venus.rotation.x = 1 / 66 * Math.PI
      this.uranus.rotation.y -= 0.007
      this.uranus.rotation.x = -0.5 * Math.PI
      this.solarGroup.rotation.y += 0.001
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

