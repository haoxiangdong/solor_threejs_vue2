<template>
    <div>
        <div id="container"></div>
    </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"
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
                    radius: 1450,
                    texture: "../assets/img/solarSystem/earth.jpg",
                    distance:0,
                    index:0,
                },
                // {
                //     name: "sx",
                //     radius: 10,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:1,
                // },
                // {
                //     name: "jx",
                //     radius: 25,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:2,
                // },
                // {
                //     name: "earth",
                //     radius: 27,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:3,
                // },
                // {
                //     name: "hx",
                //     radius: 14,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:4,
                // },
                // {
                //     name: "mx",
                //     radius: 298,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:5,
                // },
                // {
                //     name: "tx",
                //     radius: 251,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:6,
                // },
                // {
                //     name: "twx",
                //     radius: 107,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:7,
                // },
                // {
                //     name: "hwx",
                //     radius: 103,
                //     texture: "../assets/img/solarSystem/",
                //     distance:0,
                //     index:8,
                // },

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
            this.camera = new THREE.PerspectiveCamera(89, k, 1, 3000)
            this.camera.position.set(0, 0, 1800) // ??????????????????
            this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // ??????????????????
            this.scene.add(this.camera)
        },

        //??????????????????
        createMesh() {
            // for (let index = 0; index < this.geometryArray.length; index++) {
            //     const geometrySphere = new THREE.SphereGeometry(this.geometryArray[index].radius, 150, 150)//??????
            //     var texture = new THREE.TextureLoader().load(this.geometryArray[index].texture)
            //     const material = new THREE.MeshLambertMaterial({
            //     map:texture,
            // })
            //     material.side = THREE.DoubleSide
            //     const geometryPlanet = new THREE.Mesh(geometrySphere,material)
            //     geometryPlanet.rotation.x = 0.25 * Math.PI
            //     this.scene.add(geometryPlanet)
            //     console.log(index);
            // }
            // const geometryPlane = new THREE.PlaneGeometry(200, 200, 10, 10) // ????????????????????????PlaneGeometry
            // const geometryBox = new THREE.BoxGeometry(50, 50, 50)
            var texture = new THREE.TextureLoader().load("../assets/img/solarSystem/earth.jpg")
            const geometrySphere = new THREE.SphereGeometry(50, 50, 50)//??????
            const planeMaterial = new THREE.MeshLambertMaterial({
                color: 0x000099,
                map:texture
            }) // ????????????Material
            planeMaterial.side = THREE.DoubleSide//?????????????????????????????????????????????????????????????????????????????????
            const plane = new THREE.Mesh(geometrySphere, planeMaterial)
            plane.receiveShadow = true// ??????????????????????????????????????????
            plane.rotation.x = -0.25 * Math.PI
            plane.position.set(0, 0, 0)
            this.scene.add(plane)
        },
        //????????????
        createLight() {
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.8) // ???????????????
            this.scene.add(ambientLight) // ???????????????????????????
            const spotLight = new THREE.SpotLight(0x888888) // ???????????????
            spotLight.position.set(1000, 1000, 1000)
            this.scene.add(spotLight)
        },
        //???????????????
        createControls() {
            this.controls = new OrbitControls(this.camera, this.renderer.domElement) // ?????????????????????
        },
        //??????xyz?????????
        creatAxesHelper() {
            const axesHelper = new THREE.AxesHelper(250);
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