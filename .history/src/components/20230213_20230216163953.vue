<template>
    <div>
        <div id="container"></div>
    </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
import img from "../assets/img/solarSystem/earth.png"
export default {
    name: 'ModelImport20230213',

    data() {
        return {
            scene: null,
            camera: null,
            renderer: null,
            controls: null,
            geometryArray: [
                {
                    name: "sun",
                    radius: 850,
                    texture: img,
                    distance:0,
                    index:0,
                    rotation:0.004,
                },
                {
                    name: "sx",
                    radius: 100,
                    texture: img,
                    distance:2200,
                    index:1,
                    rotation:0.002,

                },
                {
                    name: "jx",
                    radius: 125,
                    texture: img,
                    distance:2900,
                    index:2,
                    rotation:0.005,

                },
                {
                    name: "earth",
                    radius: 140,
                    texture: img,
                    distance:3700,
                    index:3,
                    rotation:0.01,

                },
                {
                    name: "hx",
                    radius: 130,
                    texture: img,
                    distance:4400,
                    index:4,
                    rotation:0.01,

                },
                {
                    name: "mx",
                    radius: 298,
                    texture: img,
                    distance:5300,
                    index:5,
                    rotation:0.08,

                },
                {
                    name: "tx",
                    radius: 251,
                    texture:img,
                    distance:6400,
                    index:6,
                    rotation:1.5,

                },
                {
                    name: "twx",
                    radius: 107,
                    texture: img,
                    distance:7500,
                    index:7,
                    rotation:1,

                },
                {
                    name: "hwx",
                    radius: 103,
                    texture: img,
                    distance:8200,
                    index:8,
                    rotation:0.01,
                },

            ],


        };
    },

    mounted() {
        this.init()
    },

    methods: {
        //初始化
        init() {
            this.creatScene()//创建场景
            this.createCamera()//创建相机
            this.createRender()//创建渲染器
            this.createMesh()//创建网格模型(物体)
            this.createLight()//创建灯光    
            this.createControls()//创建控制器
            this.creatAxesHelper()//创建xyz辅助线

            // this.loadGlbModel()// 引入自定义模型

            this.render()//渲染
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
            this.camera = new THREE.PerspectiveCamera(89, k, 1, 25000)
            this.camera.position.set(0, 0, 8000) // 设置相机位置
            this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // 设置相机方向
            this.scene.add(this.camera)
        },

        //创建网格模型
        createMesh() {
            for (let index = 0; index < this.geometryArray.length; index++) {
                const geometrySphere = new THREE.SphereGeometry(this.geometryArray[index].radius, 150, 150)//球体
                var texture = new THREE.TextureLoader().load(this.geometryArray[index].texture)
                const material = new THREE.MeshLambertMaterial({
                map:texture,
            })
                material.side = THREE.DoubleSide
                const geometryPlanet = new THREE.Mesh(geometrySphere,material)
                geometryPlanet.rotation.x += 0.5* Math.PI
                geometryPlanet.position.set(this.geometryArray[index].distance,0,0)
                this.scene.add(geometryPlanet)
                console.log(index);
            }
            // //const geometryPlane = new THREE.PlaneGeometry(200, 200, 10, 10) // 创建一个平面对象PlaneGeometry
            // //const geometryBox = new THREE.BoxGeometry(50, 50, 50)
            // var texture = new THREE.TextureLoader().load(this.geometryArray[0].texture)
            // const geometrySphere = new THREE.SphereGeometry(50, 50, 50)//球体
            // const planeMaterial = new THREE.MeshLambertMaterial({
            //     map:texture
            // }) // 材质对象Material
            // planeMaterial.side = THREE.DoubleSide//修改材质使其进行双面展示（有正面、反面、双面三个属性）
            // const plane = new THREE.Mesh(geometrySphere, planeMaterial)
            // plane.receiveShadow = true// 使平面接收物体投掷过来的阴影
            // plane.rotation.x = -0.25 * Math.PI
            // plane.position.set(0, 0, 0)
            // this.scene.add(plane)
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
            const axesHelper = new THREE.AxesHelper(25000);
            axesHelper.setColors("pink", "red", "blue")//自定义辅助线颜色
            this.scene.add(axesHelper);
        },
        //创建渲染器
        createRender() {
            const element = document.getElementById('container')
            this.renderer = new THREE.WebGLRenderer()
            this.renderer.setSize(element.clientWidth, element.clientHeight) //设置渲染区域尺寸
            this.renderer.shadowMap.enabled = true // 显示阴影
            this.renderer.setClearColor(0xffffff, 1) // 设置背景颜色
            this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
            element.appendChild(this.renderer.domElement)
        },
        //引入自定义模型，固定格式
        loadGlbModel() {
            const loader = new GLTFLoader()
            loader.load('./Stork.glb', (gltf) => {
                console.log(gltf, 'gltf----->>>')
                gltf.scene.scale.set(23, 18, 20)  //设置模型大小缩放
                gltf.scene.position.set(30, 150, 80)
                let axis = new THREE.Vector3(0, 0, 0);//向量axis
                gltf.scene.rotateOnAxis(axis, Math.PI / 2);
                //绕axis轴逆旋转π/16
                gltf.scene.rotateOnAxis(axis, Math.PI / -20);
                gltf.scene.rotateOnAxis(axis, Math.PI / 50);
                this.scene.add(gltf.scene)
                return gltf.scene
            }, (xhr) => {
                console.log((xhr.loaded / xhr.total) * 100 + '% loaded')
            }, (error) => {
                console.error(error)
            })
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