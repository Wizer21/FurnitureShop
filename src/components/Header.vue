<template>
  <div id="header">
    <div id="mainPart">    
      <div id="title">
        <h1>
          Welcome
        </h1>
        <h2>
          home
        </h2>
      </div>
      <div id="start" @click="startVisit">   
        <p>
          Start Visit  
        </p>   
      </div>
    </div>    
    <div id="home" @click="backHome">
      <div class="button">
        <img :src="require('../assets/home-outline.png')">  
      </div>   
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',
  methods: {
    startVisit(){
      const mainPart = document.getElementById('mainPart')
      const home = document.getElementById('home')

      mainPart.style.transform = "translateY(-100%)"
      home.style.transform = "translateY(0%)"

      this.$emit("start")
    },
    backHome(){      
      const mainPart = document.getElementById('mainPart')
      const home = document.getElementById('home')

      mainPart.style.transform = "translateY(0%)"
      home.style.transform = "translateY(-100%)"
      
      this.$emit("home")
    }
  }
}
</script>

<style>
#header
{
  position: relative;
  height: 100vh;
  width: 100vw;

  pointer-events: none;
}
#mainPart
{
  height: 100%;
  width: 100%;

  transition-duration: 500ms;

}
#title
{
  height: 100%;
  width: 100%;

  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
#title *
{
  margin: 0;
  color: #ffffff;
  font-size: 13vw;
}
#title h1
{
  margin-right: 15%;
}
#title h2
{
  margin-left: 15%;
}
#start
{
  position: absolute;
  top: 50%;
  left: 20%;

  pointer-events: all;
  cursor: pointer;

  transition-duration: 500ms;
}
#start:active
{
  transition-duration: 50ms;
  transform: scale(0.8);
}
#start p
{  
  margin: 0;
  color: #ffffff;
  font-size: 3vw;
}
#home
{  
  position: absolute;
  top: 10px;
  left: 10px;

  transition-duration: 500ms;
  transform: translateY(-100%);
}
#home p
{
  margin: 0;
  color: #ffffff;
  font-size: 3vw;
}
</style>