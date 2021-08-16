<template>
  <div id="app">
    <WaterBall />
    <WaterBall type="horizontal" />
    <WaterBall type="horizontal" :percentage="percentage" :padding="8">
      <div class="demo-center">
        <p>占比</p>
        <p>{{percentage * 100 + "%"}}</p>
      </div>
    </WaterBall>
  </div>
</template>

<script>
import WaterBall from './components/WaterBall.vue';

export default {
  name: 'App',
  data() {
    return {
      percentage: 0.3
    };
  },
  components: {
    WaterBall
  }
};
</script>

<style scoped>
.demo-center {
  text-align: center;
  padding-top: 50px;
}
</style>
