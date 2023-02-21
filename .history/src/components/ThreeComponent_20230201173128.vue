<template>
  <div>
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"

export default {
  name: 'ThreeComponent',
  props: {
    msg: String
  },
  data() {
    return {
    };
  },
  methods: {
    init() {
      // 创建场景
      const scene = new THREE.Scene();
      let width = window.innerWidth; //窗口宽度
      let height = window.innerHeight; //窗口高度
      let k = width / height; //窗口宽高比
      let s = 2000; //三维场景显示范围控制系数，系数越大，显示的范围越大
      // 创建相机
      const camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
      // 设置相机位置
      camera.position.set(0, 0, 0);
      // 为场景添加相机
      scene.add(camera);

      // 添加物体
      // 创建几何体对象
      var geometry = new THREE.SphereGeometry(60,40,40);
      // 材质与颜色
      const material = new THREE.MeshLambertMaterial({
        color: 'ffffff',
        side: THREE.DoubleSide,
      }); //材质对象Material 可以自定义颜色
      // 根据几何体与材质创建物体
      const cube = new THREE.Mesh(geometry, material);
      //使用set方法修改物体的位置
      // cube.position.set(5, 0, 0);
      cube.position.setY(5);
      // 将几何体添加到场景
      scene.add(cube);
      cube.scale.set(3, 2, 1);
      cube.scale.x = 2;
      cube.rotation.set(Math.PI / 4, 10, 10, "YXZ");
      // 创建点光源
      const point = new THREE.PointLight(0xdee1e6);
      // 光源的位置
      point.position.set(400, 200, 300)
      scene.add(point);
      //环境光
      const ambient = new THREE.AmbientLight(0x444444);
      scene.add(ambient);


      // 初始化渲染器
      const renderer = new THREE.WebGL1Renderer({ antialias: true });
      // 设置背景色
      renderer.setClearColor(0xb9d3ff, 1);
      // 设置渲染的尺寸大小
      renderer.setSize(window.innerWidth, window.innerHeight);
      // 将webgl渲染的canvas内容添加到body
      document.body.appendChild(renderer.domElement);
      // 使用渲染器，通过相机将常见进行渲染
      renderer.render(scene, camera);
      // 创建轨道控制器
      const controls = new OrbitControls(camera, renderer.domElement);
      //添加坐标轴辅助器
      const axesHelper = new THREE.AxesHelper(500);
      scene.add(axesHelper);
      // 设置时钟
      const clock = new THREE.Clock();

      function render() {
        // 获取时钟运行的总时长
        let time = clock.getElapsedTime();
        //获取时间的间隔
        //   let deltaTime = clock.getDelta();
        //   console.log("获取时钟运行的总时长:" + time);
        //   console.log("获取时间的间隔" + deltaTime);
        let t = time % 5;
        cube.position.x = t * 1;
        cube.rotation.x = t * 1;
        renderer.render(scene, camera);
        // 渲染下一针的时候就会调用render函数
        requestAnimationFrame(render);
      }
      render();
      controls.update();
    },
  },
  mounted() {
    this.init()
  }
}
</script>


<style scoped>
#container {
  padding: 0;
  margin: 0;
}
</style>
