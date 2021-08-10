<template>
  <div id="app">
    <WaterBall1 />
    <WaterBall2 />
  </div>
</template>

<script>
import WaterBall from './components/index.js';

export default {
  name: 'App',
  components: {
    WaterBall1: WaterBall.WaterBall1,
    WaterBall2: WaterBall.WaterBall2
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
