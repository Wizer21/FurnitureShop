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
    camera.rotation.x -= Math.PI * 1.5
    camera.rotation.y += Math.PI

    camera.position.z -= 2
    camera.position.y += 1
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

      // Emissives materials 
      gltf.scene.children.find(child => child.name === "blueTriangle" ).material = new THREE.MeshBasicMaterial({color: 0x0CAAFF}) 
      gltf.scene.children.find(child => child.name === "greenTriangle" ).material = new THREE.MeshBasicMaterial({color: 0x73F0E0}) 
      gltf.scene.children.find(child => child.name === "pinkTriangle" ).material = new THREE.MeshBasicMaterial({color: 0xA349A4}) 
      gltf.scene.children.find(child => child.name === "pinkTriangle" ).material = new THREE.MeshBasicMaterial({color: 0xA349A4}) 

      gltf.scene.children.find(child => child.name === "lightYellow" ).material = new THREE.MeshBasicMaterial({color: 0xFAEE94}) 
      gltf.scene.children.find(child => child.name === "lightYellow1" ).material = new THREE.MeshBasicMaterial({color: 0xFAEE94})

      gltf.scene.children.find(child => child.name === "lightLamp" ).material = new THREE.MeshBasicMaterial({color: 0xF5F5F5}) 

      // Wall
      gltf.scene.children.find(child => child.name === "wall" ).material = new THREE.MeshBasicMaterial({color: 0x010101})

      // Glass
      gltf.scene.children.find(child => child.name === "glass" ).material = new THREE.MeshBasicMaterial({
        color: 0x010101,
        transparent: true, 
        opacity: 0.7
      })

      gltf.scene.scale.set(0.1, 0.1, 0.1)
      scene.add(gltf.scene)
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

    window.addEventListener('resize', () =>
    {
      // Update camera
      camera.aspect = window.innerWidth / window.innerHeight
      camera.updateProjectionMatrix()

      // Update renderer
      renderer.setSize(window.innerWidth, window.innerHeight)
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    })
  }
}
</script>

<style scoped>
#scene
{
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  z-index: -1;
}
</style>
