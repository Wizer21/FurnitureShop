<template>
  <Header @start="start" @home="home" />
  <Furniture ref="refFurniture" />
  <Scene />
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
    }
  }
}
</script>

<style>
body
{
  margin: 0;
  overflow: hidden;
}
</style>
