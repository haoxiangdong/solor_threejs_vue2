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
            boxMorphTargets: [
                {
                    "name": "target1",
                    "vertices": [
                        {
                            "x": 50,
                            "y": 2.5,
                            "z": 50
                        },
                        {
                            "x": 50,
                            "y": 2.5,
                            "z": -50
                        },
                        {
                            "x": 50,
                            "y": -2.5,
                            "z": 50
                        },
                        {
                            "x": 50,
                            "y": -2.5,
                            "z": -50
                        },
                        {
                            "x": -50,
                            "y": 2.5,
                            "z": -50
                        },
                        {
                            "x": -50,
                            "y": 2.5,
                            "z": 50
                        },
                        {
                            "x": -50,
                            "y": -2.5,
                            "z": -50
                        },
                        {
                            "x": -50,
                            "y": -2.5,
                            "z": 50
                        }
                    ]
                },
                {
                    "name": "target2",
                    "vertices": [
                        {
                            "x": 2.5,
                            "y": 100,
                            "z": 2.5
                        },
                        {
                            "x": 2.5,
                            "y": 100,
                            "z": -2.5
                        },
                        {
                            "x": 2.5,
                            "y": -100,
                            "z": 2.5
                        },
                        {
                            "x": 2.5,
                            "y": -100,
                            "z": -2.5
                        },
                        {
                            "x": -2.5,
                            "y": 100,
                            "z": -2.5
                        },
                        {
                            "x": -2.5,
                            "y": 100,
                            "z": 2.5
                        },
                        {
                            "x": -2.5,
                            "y": -100,
                            "z": -2.5
                        },
                        {
                            "x": -2.5,
                            "y": -100,
                            "z": 2.5
                        }
                    ]
                }
            ]

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
            this.createMesh()//??????????????????
            this.createLight()//????????????    
            this.createRender()//???????????????
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
            this.camera = new THREE.PerspectiveCamera(35, k, 0.1, 1000)
            this.camera.position.set(300, 300, 500) // ??????????????????
            this.camera.lookAt(new THREE.Vector3(10, 0, 0)) // ??????????????????
            this.scene.add(this.camera)
        },

        //??????????????????
        createMesh() {
            // const geometryPlane = new THREE.PlaneGeometry(200, 200, 10, 10) // ????????????????????????PlaneGeometry
            const geometryBox = new THREE.BoxGeometry(50, 50, 50)
            // const geometrySphere = new THREE.SphereGeometry(50, 50, 50)//??????
            console.log(geometryBox);
            geometryBox.morphTargets = this.boxMorphTargets
            var box1 = new THREE.BoxGeometry(100, 5, 100); //???????????????1????????????
            console.log(box1);
            var box2 = new THREE.BoxGeometry(5, 200, 5); //???????????????2????????????
            console.log(box2);
            // geometryBox.morphTargets[0] = {name: 'target1',vertices: box1.vertices};
            // geometryBox.morphTargets[1] = {name: 'target2',vertices: box2.vertices};
            console.log(geometryBox.morphTargets);
            const planeMaterial = new THREE.MeshLambertMaterial({
                color: 0x000099,
                morphTargets:true,//????????????
            }) // ????????????Material
            planeMaterial.side = THREE.DoubleSide//?????????????????????????????????????????????????????????????????????????????????
            const plane = new THREE.Mesh(geometryBox, planeMaterial)
            plane.receiveShadow = true// ??????????????????????????????????????????
            plane.rotation.x = -0.25 * Math.PI
            plane.position.set(25, 50, 25)
            this.scene.add(plane)
            plane.morphTargetInfluences[0] = 0.5;
            plane.morphTargetInfluences[1] = 1;
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