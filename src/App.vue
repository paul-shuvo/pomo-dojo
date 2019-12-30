<template>
  <div id="app">
    <Menubar />
    <img alt="Vue logo" src="./assets/logo.png" id="logo">
    <timer msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Timer from './components/Timer.vue'
import Menubar from './components/Menubar.vue'

export default {
  name: 'app',
  components: {
    Timer, 
    Menubar
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;  
  height: 100%;
  width: 100%;
}

#logo {
  width: 100px;
  height: 100px;
}

::-webkit-scrollbar {
    display: none;
}


</style>
