<template>
  <canvas id="scene">
  </canvas>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

export default {
  name: 'Scene',
  mounted(){
    const canvas = document.getElementById('scene')

    const scene = new THREE.Scene()

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 20)
    camera.position.set(0, -2, 0) 
    scene.add(camera)

    const light = new THREE.AmbientLight( 0xffffff );
    scene.add(light);

    // Mesh    
    // GLTF loader
    const gltfLoader = new GLTFLoader()

    // Texture
    const textureLoader = new THREE.TextureLoader()
    const bakedTexture = textureLoader.load("./bake.jpg")
    bakedTexture.flipY = false
    bakedTexture.encoding = THREE.sRGBEncoding

    // Materials 
    const bakedMaterial = new THREE.MeshBasicMaterial({ map: bakedTexture })  

    gltfLoader.load('./house.glb', gltf => {
      gltf.scene.traverse((child) =>
      {
        child.material = bakedMaterial
      })
      scene.add(gltf.scene)
      gltf.scene.scale.set(0.1, 0.1, 0.1)
    })
    
    const control = new OrbitControls(camera, canvas)
    control.enableDamping = true

    const renderer = new THREE.WebGLRenderer({
      canvas: canvas
    })
    
    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.outputEncoding = THREE.sRGBEncoding
  
    //const clock = new THREE.Clock()
    const tick = () =>
    {
      //const elapsedTime = clock.getElapsedTime()

      // Update controls
      control.update()

      // Call tick again on the next frame
      window.requestAnimationFrame(tick)

      // Render
      renderer.render(scene, camera)
    }
    tick()

    // window.addEventListener('resize', () =>
    // {
    //     // Update sizes
    //     sizes.width = window.innerWidth
    //     sizes.height = window.innerHeight

    //     // Update camera
    //     camera.aspect = sizes.width / sizes.height
    //     camera.updateProjectionMatrix()

    //     // Update renderer
    //     renderer.setSize(sizes.width, sizes.height)
    //     renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    // })
  }
}
</script>

<style scoped>
#scene
{
  position: relative;
  height: 100vh;
  width: 100vw;
}
</style>
