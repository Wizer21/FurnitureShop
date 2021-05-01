<template>
  <Header @start="start" @home="home"/>
  <Furniture ref="refFurniture" />
  <Scene @camera="camera" />
</template>

<script>
import Scene from './components/Scene.vue'
import Header from './components/Header.vue'
import Furniture from './components/Furniture.vue'

export default {
  name: 'App',
  components: { Scene, Header, Furniture },
  methods: {
    start(){
      this.$refs.refFurniture.start()
    },
    home(){     
      this.$refs.refFurniture.back()
    },
    camera(camera){   
      this.$refs.refFurniture.copyCamera(camera)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Antonio:wght@100&family=Playfair+Display&display=swap');
::-moz-selection {
  color: rgb(44, 44, 44);
  background: rgb(236, 236, 236);
}
::selection {
  color: rgb(44, 44, 44);
  background: rgb(236, 236, 236);
}
body
{
  margin: 0;
  overflow: hidden;
  font-family: 'Antonio', sans-serif;
}
.button
{
  pointer-events: all;
  height: 10vh;
  width: 10vh;
  margin: 2vh;
  border-radius: 20px;
  cursor: pointer;
  background-color: rgba(17, 17, 17, 0.2);

  transition-duration: 500ms;
}
.button img
{
  height: 100%;
  width: 100%;

  object-fit: contain;
}
.button:hover
{
  background-color: rgba(17, 17, 17, 0.8);
}
.button:active
{
  transition-duration: 50ms;
  transform: scale(0.8);
}
</style>
