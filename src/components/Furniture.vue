<template>
  <div id="furniture">
    <div id="itemData">
      <p id="itemTitle">
        {{ itemName }}
      </p>
    </div>
    <div id="navigator">
      <div class="button" @click="previous" id="previous">
        <img :src="require('../assets/icons/chevron-left.png')">  
      </div>   
      <div class="button" @click="next" id="next">
        <img :src="require('../assets/icons/chevron-right.png')">  
      </div>
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
          name: "",
          cameraPosition: [-1.055, 0.356, -0.595],
          cameraRotation: [0.13, 3.537, -0.014] 
        },
        {
          name: "Triangle Led",
          cameraPosition: [-0.444, 0.845, -0.137],
          cameraRotation: [0.077, 3.441, -0.063] 
        },
        {
          name: "Tube Lamp",
          cameraPosition: [-0.902, 0.478, 0.402],
          cameraRotation: [0.103, 2.626, 0] 
        },
        {
          name: "The perfect coffee table",
          cameraPosition: [-1.198, 0.32, -0.493],
          cameraRotation: [0.352, 4.277, 0.188] 
        },
        {
          name: "Small Stool",
          cameraPosition: [-0.079, 0.212, 0.2],
          cameraRotation: [-0.489, 1.567, 0.492]  
        },
        {
          name: "Magic Book",
          cameraPosition: [0.027, 0.150, -0.132],
          cameraRotation: [0.077, -1.972, -0.017]  
        },
        {
          name: "Simple Lamp",
          cameraPosition: [1.034, 0.485, -0.159],
          cameraRotation: [-0.388, -3.14, -0.131]  
        }
      ],
      itemPos: 1,
      itemName: "NONE",
      camera: null,
      isInNavigation: false
    }
  },
  methods: {
    copyCamera(camera){
      this.camera = camera    

      this.itemPos = 0
      this.upadteData()  
    },
    start(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(-100%)" 

      this.itemPos = 1
      this.isInNavigation = true
      this.upadteData()
    },
    back(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(0%)"

      this.itemPos = 0
      this.isInNavigation = false
      this.upadteData()
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
      if (!this.camera){
        return
      }
      // Title
      const itemTitle = document.getElementById('itemTitle')
      itemTitle.style.clipPath = "polygon(0 0, 100% 0, 100% 0, 0 0)"
      setTimeout(() => {
        itemTitle.style.transitionDuration = "0ms"
        itemTitle.style.clipPath = "polygon(0 100%, 100% 100%, 100% 100%, 0 100%)"
        this.itemName = this.furniture[this.itemPos].name

        setTimeout(() => {
          itemTitle.style.transitionDuration = "500ms"
          itemTitle.style.clipPath = "polygon(0 0, 100% 0, 100% 100%, 0% 100%)"
        }, 10)
      }, 500)

      // camera
      const pos = this.furniture[this.itemPos].cameraPosition
      const rotatiton = this.furniture[this.itemPos].cameraRotation

      gsap.to(this.camera.position, {x: pos[0], duration: 1, ease: "power2" })
      gsap.to(this.camera.position, {y: pos[1], duration: 1, ease: "power2" })
      gsap.to(this.camera.position, {z: pos[2], duration: 1, ease: "power2" })

      gsap.to(this.camera.rotation, {x: rotatiton[0], duration: 1, ease: "power2" })
      gsap.to(this.camera.rotation, {y: rotatiton[1], duration: 1, ease: "power2" })
      gsap.to(this.camera.rotation, {z: rotatiton[2], duration: 1, ease: "power2" })

      // Buttons 
      const previous = document.getElementById('previous')
      const next = document.getElementById('next')

      if (this.itemPos == 0){
        previous.style.opacity = 0
        previous.style.pointerEvents = "none"
      }
      else if(this.itemPos == 6){
        next.style.opacity = 0
        next.style.pointerEvents = "none"
      }
      else{
        previous.style.opacity = 1
        previous.style.pointerEvents = "all"
        next.style.opacity = 1
        next.style.pointerEvents = "all"
      }
    },
    updateBasePosition(){
      if(window.innerWidth < 400){
        this.furniture[0].cameraPosition[2] = -1.2
        this.upadteData()
      }
      else if(window.innerWidth < 500){
        this.furniture[0].cameraPosition[2] = -1
        this.upadteData()
      }
      else if (window.innerWidth < 750){
        this.furniture[0].cameraPosition[2] = -0.8
        this.upadteData()
      }    
      else{
        this.furniture[0].cameraPosition[2] = -0.595
        this.upadteData()
      }
    }
  },
  mounted(){
    this.updateBasePosition()

    window.addEventListener("resize", () => { this.updateBasePosition() })

    const local = this
    document.onkeydown = function (event) {
      if (local.isInNavigation){
        if (event.code == "ArrowLeft"){
          if (local.itemPos != 0){
            local.previous()
          }
        }
        else if(event.code == "ArrowRight"){
          if (local.itemPos != 6){
            console.log("in");
            local.next()
          }
        }
      }
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
  pointer-events: none;
}
#itemTitle
{
  color: white;
  font-size: 5vw;
  margin-top: 40vh;
  margin-left: 10vw;
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
#itemTitle
{
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0% 100%);
  transition-duration: 500ms;
}

@media screen and (max-width: 800px) {  
  #itemTitle
  {
    font-size: 10vw;
    margin-top: 60vh;
  }
}
</style>