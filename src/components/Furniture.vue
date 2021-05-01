<template>
  <div id="furniture">
    <div id="itemData">
      <p>
        {{ itemName }}
      </p>
    </div>
    <div id="navigator">
      <p @click="previous">
        l
      </p>
      <p @click="next">
        r
      </p>
    </div>
  </div>
</template>

<script>
import gsap from "gsap";

export default {
  name: 'Furniture',
  data(){
    return{
      furniture: [
        {
          name: "Triangle Led",
          cameraPosition: [-0.595, 0.845, -0.137],
          cameraRotation: [0.077, 3.441, -0.063] 
        },
        {
          name: "Tube Lamp",
          cameraPosition: [-0.902, 0.478, 0.402],
          cameraRotation: [0.103, 2.626, 0] 
        },
        {
          name: "Simple Table",
          cameraPosition: [-1.198, 0.32, -0.493],
          cameraRotation: [0.352, 4.277, 0.188] 
        },
        {
          name: "Stool",
          cameraPosition: [-0.079, 0.212, 0.243],
          cameraRotation: [-0.489, 1.567, 0.492]  
        }
      ],
      itemPos: 0,
      itemName: "NONE",
      camera: null
    }
  },
  methods: {
    copyCamera(camera){
      this.camera = camera      
    },
    start(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(-100%)"

      this.itemPos = 0
      this.upadteData()
    },
    back(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(0%)"
    },
    previous(){
      this.itemPos--
      this.upadteData()

    },
    next(){
      this.itemPos++
      this.upadteData()
    },
    upadteData(){
      this.itemName = this.furniture[this.itemPos].name

      const pos = this.furniture[this.itemPos].cameraPosition
      const rotatiton = this.furniture[this.itemPos].cameraRotation

      gsap.to(this.camera.position, {x: pos[0], duration: 1 })
      gsap.to(this.camera.position, {y: pos[1], duration: 1 })
      gsap.to(this.camera.position, {z: pos[2], duration: 1 })

      gsap.to(this.camera.rotation, {x: rotatiton[0], duration: 1 })
      gsap.to(this.camera.rotation, {y: rotatiton[1], duration: 1 })
      gsap.to(this.camera.rotation, {z: rotatiton[2], duration: 1 })
    }
  }
}
</script>

<style>
#furniture
{  
  position: relative;
  height: 100vh;
  width: 100vw;

  transition-duration: 500ms;
}
#furniture p
{
  color: white;
  font-size: 5vw;
}
#navigator
{
  height: 100%;
  width: 100%;

  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
#itemData
{
  position: absolute;
  margin: 10%;
}
</style>