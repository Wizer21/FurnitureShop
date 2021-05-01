<template>
  <canvas id="scene">
  </canvas>
</template>

<script>
import * as THREE from 'three';
//import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import * as dat from 'dat.gui'

// import galaxyVertexShader from '../assets/shader/vertex.glsl'
// import galaxyFragementShader from '../assets/shader/fragement.glsl'

export default {
  name: 'Scene',
  mounted(){
    const gui = new dat.GUI({width: 1000})
    const canvas = document.getElementById('scene')

    const scene = new THREE.Scene()

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.01, 20)
    camera.rotation.x = Math.PI / 25
    camera.rotation.y += Math.PI

    camera.position.z -= 2
    camera.position.y += 1
    scene.add(camera)
    this.$emit("camera", camera)

    gui.add(camera.position, 'x').min(-3).max(3).step(0.001)
    gui.add(camera.position, 'y').min(-3).max(3).step(0.001)
    gui.add(camera.position, 'z').min(-3).max(3).step(0.001)
    gui.add(camera.rotation, 'x').min(- 3.14).max(6).step(0.001)
    gui.add(camera.rotation, 'y').min(- 3.14).max(6).step(0.001)
    gui.add(camera.rotation, 'z').min(- 3.14).max(6).step(0.001)

    const cameraCord = {
      pos: function(){
        console.log(camera.position);
        console.log(camera.rotation);
      }
    }
    gui.add(cameraCord, "pos" )

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
      gltf.scene.children.find(child => child.name === "lightLamp1" ).material = new THREE.MeshBasicMaterial({color: 0xF5F5F5}) 

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
    
    //const control = new OrbitControls(camera, canvas)

    const renderer = new THREE.WebGLRenderer({
      canvas: canvas,
      antialias: true          
    })
    
    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.outputEncoding = THREE.sRGBEncoding

    // Galaxy
    const geometry = new THREE.BufferGeometry()
    const parameters = {}
    parameters.count = 50000
    parameters.radius = 0.2
    parameters.branches = 5
    parameters.spin = 1
    parameters.randomness = 0.5
    parameters.randomnessPower = 3
    parameters.insideColor = '#ff6030'
    parameters.outsideColor = '#1b3984'

    const positions = new Float32Array(parameters.count * 3)
    const colors = new Float32Array(parameters.count * 3)
    const scale = new Float32Array(parameters.count)
    const randomness = new Float32Array(parameters.count * 3)

    const insideColor = new THREE.Color(parameters.insideColor)
    const outsideColor = new THREE.Color(parameters.outsideColor)

    for(let i = 0; i < parameters.count; i++)
    {
      const i3 = i * 3

      // Position
      const radius = Math.random() * parameters.radius

      const branchAngle = (i % parameters.branches) / parameters.branches * Math.PI * 2

      positions[i3    ] = Math.cos(branchAngle) * radius + 0
      positions[i3 + 1] = 0
      positions[i3 + 2] = Math.sin(branchAngle) * radius + 0

      // Color
      const mixedColor = insideColor.clone()
      mixedColor.lerp(outsideColor, radius / parameters.radius)

      colors[i3    ] = mixedColor.r
      colors[i3 + 1] = mixedColor.g
      colors[i3 + 2] = mixedColor.b

      // Randomness        
      const randomX = Math.pow(Math.random(), parameters.randomnessPower) * (Math.random() < 0.5 ? 1 : - 1) * parameters.randomness * radius
      const randomY = Math.pow(Math.random(), parameters.randomnessPower) * (Math.random() < 0.5 ? 1 : - 1) * parameters.randomness * radius
      const randomZ = Math.pow(Math.random(), parameters.randomnessPower) * (Math.random() < 0.5 ? 1 : - 1) * parameters.randomness * radius

      randomness[i3    ] = randomX
      randomness[i3 + 1] = randomY
      randomness[i3 + 2] = randomZ

      // Size
      scale[i] = Math.random()
    }

    geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3))
    geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3))
    geometry.setAttribute('aScale', new THREE.BufferAttribute(scale, 1))
    geometry.setAttribute('aRandomness', new THREE.BufferAttribute(randomness, 3))

    /**
     * Material
     */
    const material = new THREE.ShaderMaterial({
      depthWrite: false,
      blending: THREE.AdditiveBlending,
      vertexColors: true,
      vertexShader: `
      uniform float uSize;
      uniform float uTime;

      attribute vec3 aRandomness;
      attribute float aScale;
      varying vec3 vColor;

      void main(){
        // Position
        vec4 modelPosition = modelMatrix * vec4(position, 1.0);

        // Spin
        float angle = atan((modelPosition.x), modelPosition.z);
        float distanceToCenter = length(modelPosition.xz);
        float angleOffset = (1.0 / distanceToCenter) * uTime * 0.2;
        angle += angleOffset;
        modelPosition.x = cos(angle) * distanceToCenter;
        modelPosition.z = sin(angle) * distanceToCenter;

        modelPosition.x -= 0.939;
        modelPosition.z -= 0.208;


        // Randomness
        modelPosition.xyz += aRandomness;

        
        vec4 viewPosition = viewMatrix * modelPosition;
        vec4 projectedPosition = projectionMatrix * viewPosition;
        gl_Position = projectedPosition;

        //Size
        gl_PointSize = uSize * aScale;

        // SizeAttenuation to distance from the camera  
        gl_PointSize *= (1.0 / - viewPosition.z);
        vColor = color;
      }
      `,
      fragmentShader: `
      varying vec3 vColor;

      void main(){
        //Diffuse  
        float strenght = distance(gl_PointCoord, vec2(0.5));
        strenght = 1.0 - strenght;
        strenght = pow(strenght, 5.0);

        // Final Color
        vec3 color = mix(vec3(0.0), vColor, strenght);

        gl_FragColor = vec4(color, 1.0);
      }
      `,
      uniforms: 
      {
        uTime: { value: 0 },
        uSize: { value: 2 * renderer.getPixelRatio()}
      }
    })

    /**
     * Points
     */
    const points = new THREE.Points(geometry, material)
    points.position.y = 0.3
    scene.add(points)


    const clock = new THREE.Clock()
    const tick = () =>
    {
      const elapsedTime = clock.getElapsedTime()

      //Update controls
      //control.update()

      // Animate Galaxy
      material.uniforms.uTime.value = elapsedTime * .03

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
