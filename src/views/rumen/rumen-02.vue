<template>
  <div id="content">
    <div class="container" ref="container"></div>
  </div>
</template>
<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
// 引入性能检测插件
import Stats from 'three/examples/jsm/libs/stats.module'
export default {
  mounted() {
    const scene = new THREE.Scene()
    const width = this.$refs.container.clientWidth
    const height = this.$refs.container.clientHeight

    // 创建几何体
    const geometry = new THREE.BoxGeometry(100, 100, 100)
    const material = new THREE.MeshLambertMaterial({ color: 0x00ffff })
    const mesh = new THREE.Mesh(geometry, material)
    scene.add(mesh)

    // 添加辅助线
    const axesHelper = new THREE.AxesHelper(100)
    scene.add(axesHelper)

    // 创建相机
    const camera = new THREE.PerspectiveCamera(100, width/height, 0.1, 2000)
    camera.position.set(100, 100, 100)
    camera.lookAt(0, 0, 0)
    scene.add(camera)

    // 创建光源
    const pointLight = new THREE.PointLight(0xffffff, 1.0)
    pointLight.position.set(100, 200, 100)
    scene.add(pointLight)

    // 创建渲染器
    const renderer = new THREE.WebGLRenderer()
    renderer.setSize(width, height)
    this.$refs.container.appendChild(renderer.domElement)
    renderer.render(scene, camera)

    // 添加相控件
    const controls = new OrbitControls(camera, renderer.domElement)
    controls.addEventListener('change', () => {
      renderer.render(scene, camera)
    })

    // 检测性能
    const stats = new Stats()
    document.getElementById('content').appendChild(stats.dom)
    // 添加旋转动画
    const animate = () => {
      stats.update()
      mesh.rotateZ(0.01)
      renderer.render(scene, camera)
      requestAnimationFrame(animate)
    }
    animate()

    // 监听窗口变化
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()
      renderer.setSize(window.innerWidth, window.innerHeight)
    })
  },
  data() {
    return {

    }
  },
  methods: {

  },
}
</script>
<style scoped>
.container {
  width: 100%;
  height: 100vh; /* 设置高度为视口高度 */
  background-color: #fff;
}
</style>