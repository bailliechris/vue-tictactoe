<template>
  <div id="app">
    <Header />
    <Game />
  </div>
</template>

<script>
import Game from './components/Game.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Game,
    Header
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
