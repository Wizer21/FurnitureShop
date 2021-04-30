<template>
  <div id="furniture">
    <p>
      NAVIGATION
    </p>
  </div>
</template>

<script>
export default {
  name: 'Furniture',
  methods: {
    start(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(-100%)"
    },
    back(){
      const furniture = document.getElementById('furniture')
      furniture.style.transform = "translateY(0%)"
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
  display: none;
}
#furniture p
{
  color: white;
  font-size: 5vw;
}
</style>