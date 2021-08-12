<template>
  <div id="app">
    <WaterBall />
    <WaterBall type="horizontal" />
  </div>
</template>

<script>
import WaterBall from './components/WaterBall.vue';

export default {
  name: 'App',
  components: {
    WaterBall
  }
};
</script>
