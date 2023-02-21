<template>
  <div>
    <div id="container"></div>
    <div class="controls-box">
      <section>
        <el-row>
          <el-checkbox v-model="properties.visible">visible</el-checkbox>
        </el-row>
        <el-row>
          <el-checkbox v-model="properties.transparent">transparent</el-checkbox>
        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label> emissive</label></el-col>
          <el-col :span="16">
            <div @click="inputClick">
              <el-input :value="properties.color"></el-input>
            </div>
            <div v-show="isShowColors" class="color-select-layer">
              <sketch-picker v-model="properties.color" @input="colorChange"></sketch-picker>
            </div>
          </el-col>
        </el-row>
        <el-row>
          <div v-for="(item, key) in properties" :key="key">
            <div v-if="item && item.name != undefined">
              <el-col :span="8">
                <span class="vertice-span">{{ item.name }}</span>
              </el-col>
              <el-col :span="13">
                <el-slider v-model="item.value" :min="item.min" :max="item.max" :step="item.step"
                  :format-tooltip="formatTooltip"></el-slider>
              </el-col>
              <el-col :span="3">
                <span class="vertice-span">{{ item.value }}</span>
              </el-col>
            </div>
          </div>

        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label>side</label></el-col>
          <el-col :span="16">
            <el-select v-model="properties.side" placeholder="请选择">
              <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label>mesh</label></el-col>
          <el-col :span="16">
            <el-select v-model="properties.selectMesh" placeholder="请选择">
              <el-option v-for="item in selectMeshOptions" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-col>
        </el-row>
      </section>

    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { Sketch } from 'vue-color'
import img from "../assets/img/earth.png"
import atmosphereImg from "../assets/img/atmosphereImg.jpg"
export default {
  components: {
    'sketch-picker': Sketch
  },
  data() {
    return {
      options: [
        {
          value: 'front',
          label: 'front'
        },
        {
          value: 'back',
          label: 'back'
        },
        {
          value: 'double',
          label: 'double'
        }
      ],
      selectMeshOptions: [
        {
          value: 'cube',
          label: 'cube'
        },
        {
          value: 'sphere',
          label: 'sphere'
        },
        {
          value: 'plane',
          label: 'plane'
        }
      ],

      properties: {
        opacity: {
          name: 'opacity',
          value: 0.3,
          min: 0,
          max: 1,
          step: 0.1
        },
        color: '#194977',
        selectMesh: 'sphere',
        side: 'double',
        transparent: false,
        wrapAround: false,
        visible: true
      },
      isShowColors: false,
      cube: null,
      sphere: null,
      plane: null,
      activeMesh: null,
      meshMaterial: null,
      camera: null,
      scene: null,
      renderer: null,
      controls: null
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    formatTooltip(val) {
      return val
    },
    inputClick() {
      this.isShowColors = !this.isShowColors
    },
    colorChange(val) {
      this.properties.color = val.hex
    },

    // 初始化
    init() {
      this.createScene() // 创建场景
      this.createMesh() // 创建网格模型
      this.createLight() // 创建光源
      this.createCamera() // 创建相机
      this.createRender() // 创建渲染器
      this.createControls() // 创建控件对象
      this.render() // 渲染
    },
    // 创建场景
    createScene() {
      this.scene = new THREE.Scene()
    },
    // 创建网格模型
    createMesh() {
      const geometry = new THREE.PlaneGeometry(150, 200, 10, 10) // 创建一个平面对象PlaneGeometry
      const planeMaterial = new THREE.MeshLambertMaterial({
        color: 0x777777
      }) // 材质对象Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true

      // 设置平面位置
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(0, -20, 0)

      // 平面对象添加到场景中
      // this.scene.add(plane)
      const sphereGeometry = new THREE.SphereGeometry(14, 30, 30)
      const atmosphereGeometry = new THREE.SphereGeometry(20, 30, 30)
      const cubeGeometry = new THREE.BoxGeometry(15, 15, 15)
      const planeGeometry = new THREE.PlaneGeometry(14, 14, 4, 4)

      //从外部读取贴图
      var texture = new THREE.TextureLoader().load(img)
      var atmosphereTexture = new THREE.TextureLoader().load(atmosphereImg)
      // 创建材质
      this.meshMaterial = new THREE.MeshPhongMaterial({
        map:texture
      })
        // 创建大气材质
       const atmosphereMeshMaterial = new THREE.MeshPhongMaterial({
        map:atmosphereTexture,
        color:'#1b95f0'
      })
      atmosphereMeshMaterial.transparent=true
      atmosphereMeshMaterial.opacity=0.1



      // 创建球、方块、平面
      this.sphere = new THREE.Mesh(sphereGeometry, this.meshMaterial)
      const atmosphere = new THREE.Mesh(atmosphereGeometry, atmosphereMeshMaterial)
      this.cube = new THREE.Mesh(cubeGeometry, this.meshMaterial)
      this.plane = new THREE.Mesh(planeGeometry, this.meshMaterial)

      this.sphere.position.set(0,0,0)
      this.cube.position = this.sphere.position
      this.plane.position = this.sphere.position
      this.activeMesh = this.sphere
      this.scene.add(atmosphere)
    // })

    },

    // 创建光源
    createLight() {
      // 环境光
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.1) // 创建环境光
      this.scene.add(ambientLight) // 将环境光添加到场景

      const spotLight = new THREE.SpotLight(0xffffff) // 创建聚光灯
      spotLight.position.set(-40, 60, -10)
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
      this.camera.position.set(-60, 60, 100) // 设置相机位置

      this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // 设置相机方向
      this.scene.add(this.camera)
    },
    // 创建渲染器
    createRender() {
      const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
      this.renderer.shadowMap.enabled = true // 显示阴影
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0x3f3f3f, 1) // 设置背景颜色
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
    },

    render() {
      this.updateFun()
      this.renderer.render(this.scene, this.camera)
      requestAnimationFrame(this.render)
    },
    // 创建控件对象
    createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
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

