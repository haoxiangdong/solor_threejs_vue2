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
        //?????????
        init() {
            this.creatScene()//????????????
            this.createCamera()//????????????
            this.createRender()//???????????????
            this.createMesh()//??????????????????(??????)
            this.createLight()//????????????    
            this.createControls()//???????????????
            this.creatAxesHelper()//??????xyz?????????

            // this.loadGlbModel()// ?????????????????????

            this.render()//??????
        },
        //??????3D??????
        creatScene() {
            this.scene = new THREE.Scene()
        },
        //????????????
        createCamera() {
            const element = document.getElementById('container')
            const width = element.clientWidth // ????????????
            const height = element.clientHeight // ????????????
            const k = width / height // ???????????????
            this.camera = new THREE.PerspectiveCamera(89, k, 1, 25000)
            this.camera.position.set(0, 0, 8000) // ??????????????????
            this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // ??????????????????
            this.scene.add(this.camera)
        },

        //??????????????????
        createMesh() {
            for (let index = 0; index < this.geometryArray.length; index++) {
                const geometrySphere = new THREE.SphereGeometry(this.geometryArray[index].radius, 150, 150)//??????
                var texture = new THREE.TextureLoader().load(this.geometryArray[index].texture)
                const material = new THREE.MeshLambertMaterial({
                map:texture,
            })
                material.side = THREE.DoubleSide
                const geometryPlanet = new THREE.Mesh(geometrySphere,material)
                geometryPlanet.rotation.x = 0.25 * Math.PI
                geometryPlanet.position.set(this.geometryArray[index].distance,0,0)
                geometryPlanet.rotation.y += this.geometryArray[index].rotation
                this.scene.add(geometryPlanet)
                console.log(index);
            }
            // //const geometryPlane = new THREE.PlaneGeometry(200, 200, 10, 10) // ????????????????????????PlaneGeometry
            // //const geometryBox = new THREE.BoxGeometry(50, 50, 50)
            // var texture = new THREE.TextureLoader().load(this.geometryArray[0].texture)
            // const geometrySphere = new THREE.SphereGeometry(50, 50, 50)//??????
            // const planeMaterial = new THREE.MeshLambertMaterial({
            //     map:texture
            // }) // ????????????Material
            // planeMaterial.side = THREE.DoubleSide//?????????????????????????????????????????????????????????????????????????????????
            // const plane = new THREE.Mesh(geometrySphere, planeMaterial)
            // plane.receiveShadow = true// ??????????????????????????????????????????
            // plane.rotation.x = -0.25 * Math.PI
            // plane.position.set(0, 0, 0)
            // this.scene.add(plane)
        },
        //????????????
        createLight() {
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // ???????????????
            this.scene.add(ambientLight) // ???????????????????????????
            const spotLight = new THREE.SpotLight(0x888888) // ???????????????
            spotLight.position.set(10, 100, 1000)
            this.scene.add(spotLight)
        },
        //???????????????
        createControls() {
            this.controls = new OrbitControls(this.camera, this.renderer.domElement) // ?????????????????????
        },
        //??????xyz?????????
        creatAxesHelper() {
            const axesHelper = new THREE.AxesHelper(25000);
            axesHelper.setColors("pink", "red", "blue")//????????????????????????
            this.scene.add(axesHelper);
        },
        //???????????????
        createRender() {
            const element = document.getElementById('container')
            this.renderer = new THREE.WebGLRenderer()
            this.renderer.setSize(element.clientWidth, element.clientHeight) //????????????????????????
            this.renderer.shadowMap.enabled = true // ????????????
            this.renderer.setClearColor(0xffffff, 1) // ??????????????????
            this.renderer.shadowMap.type = THREE.PCFSoftShadowMap
            element.appendChild(this.renderer.domElement)
        },
        //????????????????????????????????????
        loadGlbModel() {
            const loader = new GLTFLoader()
            loader.load('./Stork.glb', (gltf) => {
                console.log(gltf, 'gltf----->>>')
                gltf.scene.scale.set(23, 18, 20)  //????????????????????????
                gltf.scene.position.set(30, 150, 80)
                let axis = new THREE.Vector3(0, 0, 0);//??????axis
                gltf.scene.rotateOnAxis(axis, Math.PI / 2);
                //???axis??????????????/16
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
        render() { //???????????????????????????
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