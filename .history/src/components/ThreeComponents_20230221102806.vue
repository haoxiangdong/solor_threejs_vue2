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
    },
    createLight(){},
    createCamera(){},
    createRender(){},
    createControls(){
      this.controls = new OrbitControls(this.camera, this.renderer.domElement) // 控制器固定写法
    },
    creatAxesHelper(){},
    update(){},
    render(){},
  },
};
</script>

<style lang="scss" scoped>

</style>