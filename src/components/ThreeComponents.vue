<template>
  <div>
  <div id="container">
  </div>
  
    <div class="info">
      <div class="box1">
        <span>This&nbsp;is&nbsp;{{this.title}}</span>
      </div>
      <div class="box2">
        <span>{{ this.content }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
import Stats from 'three/examples/jsm/libs/stats.module.js'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import TWEEN from '@tweenjs/tween.js'
import sun_img from '../assets/solar/太阳/8k_sun.jpg'
import mercury_img from '../assets/solar/水星/8k_mercury.jpg';
import venus_img from '../assets/solar/金星/8k_venus_surface.jpg'
import earth_img from '../assets/solar/地球/8k_earth_daymap.jpg'
import mars_img from '../assets/solar/火星/8k_mars.jpg'
import jupiter_img from '../assets/solar/木星/8k_jupiter.jpg'
import saturn_img from '../assets/solar/土星/8k_saturn.jpg'
import uranus_img from '../assets/solar/天王星/2k_uranus.jpg'
import neptune_img from '../assets/solar/海王星/2k_neptune.jpg'
import background_img from '../assets/solar/8k_stars_milky_way.jpg'
export default {
  components: {
  },
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      sun: null,
      sunTxt: null,
      mercury: null,
      venus: null,
      earth: null,
      mars: null,
      jupiter: null,
      saturn: null,
      uranus: null,
      neptune: null,
      mercuryTxt: null,
      venusTxt: null,
      earthTxt: null,
      marsTxt: null,
      jupiterTxt: null,
      saturnTxt: null,
      uranusTxt: null,
      neptuneTxt: null,
      solarGroup: null,
      degree: 0,
      title:"the solar system",
      content:"The Solar System is a celestial system centered on the Sun and sustained by its gravity",
      planet: [
        {
          name: "mercury",
          radios: 15,
          img: mercury_img,
          autorotationRate: 0.1,
          starLoop: false,
          speed: 370,
          x: 170,
          y: 0,
          z: 0,
        },
        {
          name: "venus",
          radios: 24,
          img: venus_img,
          autorotationRate: 0.1,
          starLoop: false,
          speed: 520,
          x: 280,
          y: 0,
          z: 0,
        },
        {
          name: "earth",
          radios: 27,
          img: earth_img,
          autorotationRate: 0.1,
          starLoop: false,
          speed: 740,
          x: 400,
          y: 0,
          z: 0,
        },
        {
          name: "mars",
          radios: 25,
          img: mars_img,
          autorotationRate: 0.1,
          starLoop: false,
          speed: 890,
          x: 520,
          y: 0,
          z: 0,
        },
        {
          name: "jupiter",
          radios: 57,
          img: jupiter_img,
          autorotationRate: 0.05,
          starLoop: true,
          speed: 1050,
          x: 700,
          y: 0,
          z: 0,
        },
        {
          name: "saturn",
          radios: 45,
          img: saturn_img,
          autorotationRate: 0.05,
          starLoop: true,
          speed: 1240,
          x: 850,
          y: 0,
          z: 0,
        },
        {
          name: "uranus",
          radios: 34,
          img: uranus_img,
          autorotationRate: 0.07,
          starLoop: true,
          speed: 1500,
          x: 950,
          y: 0,
          z: 0,
        },
        {
          name: "neptune",
          radios: 33,
          img: neptune_img,
          autorotationRate: 0.05,
          starLoop: true,
          speed: 1690,
          x: 1050,
          y: 0,
          z: 0,
        },
      ],
      times: 0,
      clock: new THREE.Clock(),
      renderT: 1 / 60,
      stats: null,
      pointsGeometry: null,
      raycaster: new THREE.Raycaster(),
      pointer: new THREE.Vector2(),

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
      this.createPartic()
      this.createWholeScene()
      this.createCamera()
      this.createRender()
      this.createControls()
      this.creatAxesHelper()
      this.createStats()
      this.addTxt()
      this.render()

    },
    // 创建场景
    createScene() {
      this.scene = new THREE.Scene()
      // this.scene.background.dispose()

    },
    createSunMesh() {
      const geometry = new THREE.SphereBufferGeometry(120, 50, 50)
      const texture = new THREE.TextureLoader().load(sun_img)
      const material = new THREE.MeshLambertMaterial({
        map: texture,
        emissive: 0x262c30,//亮度
      }) // 材质对象Material
      this.sun = new THREE.Mesh(geometry, material)
      this.sun.receiveShadow = true
      // 设置位置
      this.sun.position.set(0, 0, 0)
      this.solarGroup.add(this.sun)
    },
    creatPlanetMesh() {
      for (let index = 0; index < this.planet.length; index++) {
        const plane_index = this.planet[index]
        const geometry = new THREE.SphereBufferGeometry(plane_index.radios, 50, 50)
        const track = new THREE.Mesh(new THREE.RingGeometry(plane_index.x, plane_index.x + 2, 200, 1), new THREE.MeshBasicMaterial({
          side: THREE.DoubleSide,
          color: 0xffffff,
        }));
        var loop;
        var loopGroup = new THREE.Group();
        const textTrueLoop = new THREE.TextureLoader().load(plane_index.img)
        if (plane_index.starLoop == true) {
          loop = new THREE.Mesh(new THREE.RingGeometry(plane_index.radios + 5, plane_index.radios + 15, 200, 1), new THREE.MeshBasicMaterial({
            side: THREE.DoubleSide,
            color: 0x80776c,
            map: textTrueLoop
          }))
          loop.rotation.x = -Math.PI / 2
          loopGroup.add(loop)
        }
        track.rotation.x = - Math.PI / 2;
        this.scene.add(track)
        const texture = new THREE.TextureLoader().load(plane_index.img)
        const material = new THREE.MeshLambertMaterial({
          map: texture,
        })
        const planets = new THREE.Mesh(geometry, material)
        loopGroup.add(planets)

        if (plane_index.name == "mercury") {
          this.mercury = loopGroup
        } else if (plane_index.name == "venus") {
          this.venus = loopGroup
        } else if (plane_index.name == "earth") {
          this.earth = loopGroup
        } else if (plane_index.name == "mars") {
          this.mars = loopGroup
        } else if (plane_index.name == "jupiter") {
          this.jupiter = loopGroup
        } else if (plane_index.name == "saturn") {
          this.saturn = loopGroup
        } else if (plane_index.name == "uranus") {
          this.uranus = loopGroup
        } else if (plane_index.name == "neptune") {
          this.neptune = loopGroup
        }
        loopGroup.receiveShadow = true
        this.solarGroup.add(loopGroup)
      }
    },
    createSolarGroup() {
      this.solarGroup = new THREE.Group();
      this.scene.add(this.solarGroup)
    },
    createStats() {
      this.stats = new Stats()
      this.stats.setMode(0)
      this.stats.domElement.style.position = 'absolute'
      this.stats.domElement.style.left = '0px'
      this.stats.domElement.style.top = '0px'
      document.body.appendChild(this.stats.domElement)
    },
    createWholeScene() {
      var material = new THREE.MeshBasicMaterial();
      var texture = new THREE.TextureLoader().load(background_img);
      material.map = texture;
      var skyBox = new THREE.Mesh(new THREE.SphereBufferGeometry(4000, 60, 60), material);
      skyBox.geometry.scale(-1, 1, 1);
      this.scene.add(skyBox)
    },
    // 创建光源
    createLight() {
      // 环境光
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // 创建环境光
      const spotLight = new THREE.SpotLight(0x888888) // 创建聚光灯
      spotLight.position.set(0, 0, 0)
      this.scene.add(ambientLight) // 将环境光添加到场景
      this.scene.add(spotLight)
    },
    // 创建相机
    createCamera() {
      const element = document.getElementById('container')
      const width = element.clientWidth // 窗口宽度
      const height = element.clientHeight // 窗口高度
      const k = width / height // 窗口宽高比
      this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 10000)
      this.camera.position.set(100, 800, 2300) // 设置相机位置
      this.camera.lookAt(new THREE.Vector3(100, 0, 0)) // 设置相机方向
      this.scene.add(this.camera)
    },
    // 创建渲染器
    createRender() {
      const element = document.getElementById('container')
      this.renderer = new THREE.WebGLRenderer({
        antialias: true,
        alpha: true
      })
      this.renderer.shadowMapSoft = true;
      this.renderer.setSize(element.clientWidth, element.clientHeight) // 设置渲染区域尺寸
      this.renderer.shadowMap.enabled = false // 显示阴影
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
      // this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色 如果设置了背景颜色，背景图片将不展示
      element.appendChild(this.renderer.domElement)
    },
    createPartic() {
      const textureLoader = new THREE.TextureLoader()
      const particleTexture = textureLoader.load('../assets/solar/star_05.png')
      // const particlesGeometry = new THREE.SphereBufferGeometry(1000, 32, 32)
      const particlesMaterial = new THREE.PointsMaterial({
        //点的大小
        size: 3,
        //开启尺寸衰减，当相机靠近时粒子变大，当相机远离时粒子变小
        sizeAttenuation: true,
        side: THREE.DoubleSide,
        alphaMap: particleTexture,
        // alphaTest:0.1,
        opactity: 1,
        // transparent :true,
        depthTest: true,
        color: 0xffffff,
      })
      // const particles = new THREE.Points(particlesGeometry, particlesMaterial)
      // particles.material.color = new THREE.Color('#bbffaa')

      // this.scene.add(particles)
      this.pointsGeometry = new THREE.BufferGeometry()
      this.pointsGeometry.tween = []
      const vertices = []

      for (let index = 0; index < 3000; index++) {
        const position = THREE.MathUtils.randFloatSpread(-2900, 2900)
        this.pointsGeometry.tween.push(new TWEEN.Tween({ position }).easing(TWEEN.Easing.Exponential.In))
        vertices.push(position)

      }
      this.pointsGeometry.setAttribute('position', new THREE.BufferAttribute(new Float32Array(vertices), 3))
      const point = new THREE.Points(this.pointsGeometry, particlesMaterial)
      this.solarGroup.add(point)
    },
    creatAxesHelper() {
      const axesHelper = new THREE.AxesHelper(250);
      axesHelper.setColors("pink", "red", "blue")
      this.scene.add(axesHelper);
    },
    createTxt(name, txtHeight) {
      let canvas = document.createElement("canvas");
      let ctx = canvas.getContext("2d");
      ctx.fillStyle = "#dee1e6";
      ctx.font = "Bold 100px Arial";
      ctx.lineWidth = 100;
      ctx.fillText(name, 4, 104);
      let texture = new THREE.Texture(canvas);
      texture.needsUpdate = true;
      //使用Sprite显示文字
      let material = new THREE.SpriteMaterial({ map: texture });
      let textObj = new THREE.Sprite(material);
      textObj.scale.set(0.5 * 100, 0.25 * 100, 0.75 * 100);
      textObj.position.z = txtHeight;
      textObj.name = name
      if (name == "sun") {
        textObj.position.z = 0
        textObj.position.y = txtHeight
      }
      return textObj
    },
    addTxt() {
      this.sunTxt = this.createTxt("sun", 140)
      this.solarGroup.add(this.sunTxt)
      for (let index = 0; index < this.planet.length; index++) {
        const indexs = this.planet[index]
        if (indexs.name == "mercury") {
          this.mercuryTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.mercuryTxt)
        } else if (indexs.name == "venus") {
          this.venusTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.venusTxt)
        } else if (indexs.name == "earth") {
          this.earthTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.earthTxt)
        } else if (indexs.name == "mars") {
          this.marsTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.marsTxt)
        } else if (indexs.name == "jupiter") {
          this.jupiterTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.jupiterTxt)
        } else if (indexs.name == "saturn") {
          this.saturnTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.saturnTxt)
        } else if (indexs.name == "uranus") {
          this.uranusTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.uranusTxt)
        } else if (indexs.name == "neptune") {
          this.neptuneTxt = this.createTxt(indexs.name, indexs.x)
          this.solarGroup.add(this.neptuneTxt)
        }
      }
    },
    onClick(event) {
      this.pointer.x = (event.clientX / window.innerWidth) * 2 - 1;
      this.pointer.y = 1 - (event.clientY / window.innerHeight) * 2;
      this.raycaster.setFromCamera(this.pointer,this.camera)
      const intersects = this.raycaster.intersectObjects(this.scene.children);
      if (intersects.length && intersects[0].object.name!="") {
            console.log(intersects[0]);
            // intersects[ 0 ].object.material.color.set( 0x808082 );
           console.log("这是"+intersects[0].object.name)
           this.title=intersects[0].object.name
        }
    },
    onMouseMove(event) {
      this.pointer.x = (event.clientX / window.innerWidth) * 2 - 1;
      this.pointer.y = 1 - (event.clientY / window.innerHeight) * 2;
      this.raycaster.setFromCamera(this.pointer,this.camera)
      const intersects = this.raycaster.intersectObjects(this.scene.children);
      if (intersects.length && intersects[0].object.name!="") {
            // intersects[ 0 ].object.material.color.set( 0x808082 );
           this.title=intersects[0].object.name
        }
    },
    update() {
      for (let index = 0; index < this.planet.length; index++) {
        const indexName = this.planet[index]
        const d = (++this.degree * Math.PI) / indexName.speed
        const x = Math.sin(d) * indexName.x
        const z = Math.cos(d) * indexName.x
        if (indexName.name == "mercury") {
          this.mercury.name="mercury"
          this.mercury.position.set(x, 0, z)
          this.mercury.rotation.y += indexName.autorotationRate
          this.mercuryTxt.position.set(x, 22, z)
        } else if (indexName.name == "venus") {
          this.venus.name="venus"
          this.venus.position.set(x, 0, z)
          this.venusTxt.position.set(x, 33, z)
        } else if (indexName.name == "earth") {
          this.earth.name="earth"
          this.earth.position.set(x, 0, z)
          this.earthTxt.position.set(x, 36, z)
        } else if (indexName.name == "mars") {
          this.mars.name="mars"
          this.mars.position.set(x, 0, z)
          this.marsTxt.position.set(x, 35, z)
        } else if (indexName.name == "jupiter") {
          this.jupiter.name="jupiter"
          this.jupiter.position.set(x, 0, z)
          this.jupiterTxt.position.set(x, 68, z)
        } else if (indexName.name == "saturn") {
          this.saturn.name="saturn"
          this.saturn.position.set(x, 0, z)
          this.saturnTxt.position.set(x, 57, z)
        } else if (indexName.name == "uranus") {
          this.uranus.name="uranus"
          this.uranus.position.set(x, 0, z)
          this.uranusTxt.position.set(x, 58, z)
        } else if (indexName.name == "neptune") {
          this.neptune.name="neptune"
          this.neptune.position.set(x, 0, z)
          this.neptuneTxt.position.set(x, 44, z)
        }
      }
      this.sun.name="sun"
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
      this.solarGroup.rotation.y -= 0.01
    },
    render() { //渲染永远在最后执行
      document.body.addEventListener('click', this.onClick, false);
      document.body.addEventListener('mousemove', this.onMouseMove, false);
      requestAnimationFrame(this.render)
      var T = this.clock.getDelta();
      this.times = this.times + T
      if (this.times > this.renderT) {
        this.renderer.render(this.scene, this.camera)
        this.update()
        this.stats.update()
        this.controls.update()
        this.times = 0
      }
    },
    // 创建控件对象
    createControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
      this.controls.enableDamping = true
      this.controls.dampingFactor = 0.2
      this.controls.autoRotate = true
      this.controls.rotateSpeed = 2 //相机的旋转速度
      this.controls.zoomSpeed = 1.0 //相机的缩放速度
      this.controls.panSpeed = 1.0 //相机的平移速度
      this.controls.minDistance = 700;
      //设置相机距离原点的最远距离
      this.controls.maxDistance = 3000;
    }
  }
}
</script>

<style>
#container {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #000 url(.././assets/solar/starry_sky_bg.jpg) no-repeat center center;
}
.info{
  width: 210px;
  height: 320px;
  position: absolute;
  top: 0;
  right:3px;
  opacity: 0.8;
  --borderWidth: 3px;
  background: #1D1F20;
  border-radius: var(--borderWidth);
}
.info:after {
  content: '';
  position: absolute;
  top: calc(-1 * var(--borderWidth));
  right: calc(-1 * var(--borderWidth));
  height: calc(100% + var(--borderWidth) * 2);
  width: calc(100% + var(--borderWidth) * 2);
  background: linear-gradient(60deg, #f79533, #f37055, #ef4e7b, #a166ab, #5073b8, #1098ad, #07b39b, #6fba82);
  border-radius: calc(2 * var(--borderWidth));
  z-index: -1;
  animation: animatedgradient 3s ease alternate infinite;
  background-size: 300% 300%;
}
.box1{
  width: 95%;
  height: 20%;
  background: #1D1F20;
  display: flex;
  margin: 3% 3% 2% 2%;
  border-radius: 10px 10px 10px 10px;
  align-items: center;
  justify-content: center;
  color: white;
  font-family: 'Raleway';
  font-size: 0.5rem;
}
.box2{
  width: 93%;
  height: 75%;
  background: #1D1F20;
  display: flex;
  margin: 3% 3% 2% 2%;
  border-radius: 10px 10px 10px 10px;
  align-items: center;
  justify-content: center;
  color: white;
  font-family: 'Raleway';
  font-size: 0.5rem;
  text-indent: 2em;
  padding-left: 5px;
}
@keyframes animatedgradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}
</style>

