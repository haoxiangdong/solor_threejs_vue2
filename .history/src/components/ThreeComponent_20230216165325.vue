<template>
  <div>
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
import { Sketch } from 'vue-color'
import img from "../assets/img/earth.jpg"
import atmosphereImg from "../assets/img/atmosphereImg.jpg"
// import airplane from "../assets/glb/747airplane"
// import background from "../assets/img/background.png"
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
          value: 1,
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
      controls: null,
      atmosphere: null,
      positionKF: null,
      clip: null,
      mixer: null,
      clips: null,
      clipAction: null,
      clock: new THREE.Clock(),
      model: null,

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

    // ?????????
    init() {

      this.createScene() // ????????????
      this.createMesh() // ??????????????????
      this.createLight() // ????????????
      this.createCamera() // ????????????
      this.createRender() // ???????????????
      this.createControls() // ???????????????
      this.loadGlbModel() // GLTFLoader
      this.loadairBusGlbModel() // GLTFLoader
      this.loadairplaneGlbModel()
      this.creatAxesHelper() // xyz????????? 
      this.creatAnimation() // ??????
      this.creatEnableAnimation() //???????????????
      this.update()//
      this.render() // ??????

    },
    loadairBusGlbModel() {
      const loader = new GLTFLoader()
      // const dracoLoader = new DRACOLoader()
      // dracoLoader.setDecoderPath('/draco/')
      // dracoLoader.preload()
      // loader.setDRACOLoader(dracoLoader)
      loader.load('./Stork.glb', (gltf) => {
        console.log(gltf, 'gltf----->>>')
        gltf.scene.scale.set(23, 18, 20)  //  ????????????????????????
        gltf.scene.position.set(30, 150, 80)
        let axis = new THREE.Vector3(0, 0, 0);//??????axis
        gltf.scene.rotateOnAxis(axis, Math.PI / 2);
        //???axis??????????????/16
        gltf.scene.rotateOnAxis(axis, Math.PI / -20);
        gltf.scene.rotateOnAxis(axis, Math.PI / 50);
        this.model = gltf.scene
        console.log(this.model);
        this.scene.add(this.model)
        return gltf.scene
      }, (xhr) => {
        console.log((xhr.loaded / xhr.total) * 100 + '% loaded')
      }, (error) => {
        console.error(error)
      })
    },
    creatAnimation() {
      // ??????
      this.positionKF = new THREE.VectorKeyframeTrack(
        ".position",
        [0, 1, 2, 3, 4, 5, 6, 7],
        [
          200, 50, 0,
          230, 0, 0,
          260, 0, 0,
          300, 0, 0,
          100, 200, 300,
          50, 1, 1,
          110, 10, -100,
          300, 300, 0
        ]
        // ????????????
      );
      this.clip = new THREE.AnimationClip(
        "Action", 10, [this.positionKF]
      ) //?????????1.?????? 2.time 3. ????????????
    },
    creatEnableAnimation() {
      console.log(this.scene);
      this.mixer = new THREE.AnimationMixer(this.activeMesh) // ?????????????????????????????????????????????????????????
      console.log(this.activeMesh);
      this.clips = this.activeMesh.animations;
      this.clipAction = this.mixer.clipAction(this.clip)
      this.clipAction.play()

      // this.mixer.update(new THREE.Clock().getDelta())
    },
    creatAxesHelper() {
      const axesHelper = new THREE.AxesHelper(250);
      axesHelper.setColors("pink", "red", "blue")
      this.scene.add(axesHelper);
    },

    
    loadairplaneGlbModel() {
      const loader = new GLTFLoader()
      // const dracoLoader = new DRACOLoader()
      // dracoLoader.setDecoderPath('/draco/')
      // dracoLoader.preload()
      // loader.setDRACOLoader(dracoLoader)
      loader.load('./airbus.glb', (gltf) => {
        console.log(gltf, 'gltf----->>>')
        gltf.scene.scale.set(23, 18, 20)  //  ????????????????????????
        gltf.scene.position.set(30, 50, 80)
        let axis = new THREE.Vector3(0, 1, 0);//??????axis
        gltf.scene.rotateOnAxis(axis, Math.PI / 2);
        //???axis??????????????/16
        gltf.scene.rotateOnAxis(axis, Math.PI / -20);
        gltf.scene.rotateOnAxis(axis, Math.PI / 50);
        // gltf.rotateY(Math.PI / 2);
        // this.groupBox.add(gltf);
        this.scene.add(gltf.scene)
      }, (xhr) => {
        console.log((xhr.loaded / xhr.total) * 100 + '% loaded')
      }, (error) => {
        console.error(error)
      })
    },
    loadGlbModel() {
      const loader = new GLTFLoader()
      // const dracoLoader = new DRACOLoader()
      // dracoLoader.setDecoderPath('/draco/')
      // dracoLoader.preload()
      // loader.setDRACOLoader(dracoLoader)
      loader.load('./747airplane.glb', (gltf) => {
        console.log(gltf, 'gltf----->>>')
        gltf.scene.scale.set(8, 5, 5)  //  ????????????????????????
        gltf.scene.position.set(0, 30, 0)
        let axis = new THREE.Vector3(0, 1, 0);//??????axis
        gltf.scene.rotateOnAxis(axis, Math.PI / 2);
        //???axis??????????????/16
        gltf.scene.rotateOnAxis(axis, Math.PI / -20);
        gltf.scene.rotateOnAxis(axis, Math.PI / 50);
        // gltf.rotateY(Math.PI / 2);
        // this.groupBox.add(gltf);
        this.scene.add(gltf.scene)
      }, (xhr) => {
        console.log((xhr.loaded / xhr.total) * 100 + '% loaded')
      }, (error) => {
        console.error(error)
      })
    },
    // ????????????
    createScene() {
      this.scene = new THREE.Scene()
    },
    // ??????????????????
    createMesh() {
      const geometry = new THREE.PlaneGeometry(200, 200, 10, 10) // ????????????????????????PlaneGeometry
      const planeMaterial = new THREE.MeshLambertMaterial({
        color: 0x777777
      }) // ????????????Material
      const plane = new THREE.Mesh(geometry, planeMaterial)
      plane.receiveShadow = true

      // ??????????????????
      plane.rotation.x = -0.5 * Math.PI
      plane.position.set(0, -50, 0)
      // plane.
      // ??????????????????????????????
      // this.scene.add(plane)
      const sphereGeometry = new THREE.SphereGeometry(14, 30, 30)
      const atmosphereGeometry = new THREE.SphereGeometry(20, 30, 30)
      const cubeGeometry = new THREE.BoxGeometry(15, 15, 15)
      const planeGeometry = new THREE.PlaneGeometry(14, 14, 4, 4)

      //?????????????????????
      var texture = new THREE.TextureLoader().load(img)
      var atmosphereTexture = new THREE.TextureLoader().load(atmosphereImg)
      // ????????????
      this.meshMaterial = new THREE.MeshPhongMaterial({
        map: texture
      })
      // ??????????????????
      const atmosphereMeshMaterial = new THREE.MeshPhongMaterial({
        map: atmosphereTexture,
        color: '#1b95f0'
      })
      atmosphereMeshMaterial.transparent = true
      atmosphereMeshMaterial.opacity = this.properties.opacity.value



      // ???????????????????????????
      this.sphere = new THREE.Mesh(sphereGeometry, this.meshMaterial)
      this.atmosphere = new THREE.Mesh(atmosphereGeometry, atmosphereMeshMaterial)
      this.cube = new THREE.Mesh(cubeGeometry, this.meshMaterial)
      this.plane = new THREE.Mesh(planeGeometry, this.meshMaterial)

      this.sphere.position.set(50, -5, 0)
      this.cube.position = this.sphere.position
      this.plane.position = this.sphere.position
      this.activeMesh = this.sphere
      this.scene.add(this.atmosphere)
      // })

    },

    // ????????????
    createLight() {
      // ?????????
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // ???????????????
      this.scene.add(ambientLight) // ???????????????????????????

      const spotLight = new THREE.SpotLight(0x888888) // ???????????????
      spotLight.position.set(10, 100, 1000)
      this.scene.add(spotLight)
    },
    // ????????????
    createCamera() {
      const element = document.getElementById('container')
      const width = element.clientWidth // ????????????
      const height = element.clientHeight // ????????????
      const k = width / height // ???????????????
      // PerspectiveCamera( fov, aspect, near, far )
      this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 1000)
      this.camera.position.set(300, 300, 500) // ??????????????????

      this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // ??????????????????
      this.scene.add(this.camera)
    },
    // ???????????????
    createRender() {
      const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(element.clientWidth, element.clientHeight) // ????????????????????????
      this.renderer.shadowMap.enabled = false // ????????????
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      this.renderer.setClearColor(0xffffff, 1) // ??????????????????
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
    // ????????????
    updateFun() {
      this.meshMaterial.opacity = this.properties.opacity.value
      this.meshMaterial.transparent = this.properties.transparent
      this.meshMaterial.emissive.setStyle(this.properties.color)
      this.meshMaterial.visible = this.properties.visible

      this.updateMesh(this.properties.selectMesh)
      this.updateSide(this.properties.side)
      this.activeMesh.rotation.y += 0.01 //??????
      this.atmosphere.rotation.y -= 0.02
      // console.log(this.delta);
      // console.log(this.mixer);
    },
    update() {
      var delta = this.clock.getDelta()
      this.mixer.update(delta)
    },
    render() { //???????????????????????????
      this.update()
      this.updateFun()
      this.renderer.render(this.scene, this.camera)
      requestAnimationFrame(this.render)
    },
    // ??????????????????
    createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // ?????????????????????
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

