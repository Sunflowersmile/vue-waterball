<template>
  <!-- <svg version="1.1" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" :viewBox="`0 0 ${size} ${size}`"> -->
  <svg version="1.1" xmlns="http://www.w3.org/2000/svg" :width="800" :height="800" :viewBox="`0 0 800 800`">
    <defs>
      <clipPath id="clipPath">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize" />
      </clipPath>
    </defs>
    <!-- <g clip-path="url(#clipPath)"> -->
    <g>
      <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth / 2" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
      <defs>
        <g id="wave">
          <path :d="pathStr" :stroke="waterColor" :fill="waterColor">
          </path>
          <!-- <animateTransform attributeName="transform" attributeType="XML" type="translate" from="0" :to="-size" :dur="duration" repeatCount="indefinite">
        </animateTransform> -->
        </g>
      </defs>
      <g class="wave-group-1">
        <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave" :x="index * waveLength" />
        <!-- <animateTransform id="firstToleft" attributeType="XML" attributeName="transform" begin="0s" :dur="getDur(groupLength)" type="translate" from="0" fill="freeze" :to="-groupLength" />
        <animateTransform id="firstLoop" attributeType="XML" attributeName="transform" begin="secondToLeft.end" :dur="getDur(groupLength)" type="translate" :from="groupLength" to="0" />
        <animateTransform attributeType="XML" attributeName="transform" begin="secondToLeft.end" :dur="getDur(groupLength * 2)" type="translate" from="0" :to="-groupLength" /> -->
      </g>
      <g class="wave-group-2" :transform="`translate(${groupLength},0)`">
        <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave" :x="index * waveLength" />
        <!-- <animateTransform id="secondToLeft" attributeType="XML" attributeName="transform" begin="0s" :dur="getDur(groupLength)" type="translate" :from="groupLength" fill="freeze" :to="0" />
        <animateTransform attributeType="XML" attributeName="transform" begin="secondToLeft.end" :dur="getDur(groupLength)" type="translate" fill="freeze" from="0" :to="-groupLength" /> -->
        <!-- <animateTransform attributeType="XML" attributeName="transform" begin="toleft.end + 3s" dur="5s" type="translate" :from="minWaveNum * waveLength" :to="-minWaveNum * waveLength" repeatCount="indefinite" /> -->
      </g>
      <!-- <use xlink:href="#wave">
        <animateTransform id="toleft" attributeType="XML" attributeName="transform" begin="0s" dur="1.5s" type="translate" from="0" fill="freeze" :to="-waveLength" />
        <animateTransform attributeType="XML" attributeName="transform" begin="toleft.end + 3s" dur="2s" type="translate" :from="size" :to="-waveLength" repeatCount="indefinite" />
      </use> -->
    </g>
  </svg>
</template>

<script>
export default {
  name: 'WaterBall',
  props: {
    size: {
      type: Number,
      default: 200
    },
    percentage: {
      type: Number,
      default: 0.36
    },
    borderWidth: {
      type: Number,
      default: 5
    },
    borderColor: {
      type: String,
      default: 'red'
    },
    backgroundColor: {
      type: String,
      default: '#FFF'
    },
    waterColor: {
      type: String,
      default: '#F00'
    },
    speed: {
      // px/s
      type: Number,
      default: 20
    },
    amplitude: {
      // 振幅
      type: Number,
      default: 0.1
    },
    waveLength: {
      // 波长
      type: Number,
      default: 120
    }
  },
  computed: {
    halfSize() {
      return this.size / 2;
    },
    minWaveNum() {
      return Math.ceil(this.size / this.waveLength);
    },
    groupLength() {
      return this.minWaveNum * this.waveLength;
    },
    pathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        amplitude = Math.floor(this.amplitude * halfSize),
        waveLength = this.waveLength,
        halfWaveLength = Math.floor(waveLength / 2),
        quarterWaveLength = Math.floor(waveLength / 4);

      let lineY = Math.floor(size * (1 - percentage));

      let str = `m 0 ${lineY} q ${quarterWaveLength} -${amplitude} ${halfWaveLength} 0  t ${halfWaveLength} 0 v ${size - lineY} h -${waveLength} V`;
      console.log(str);
      return str;
    },
    getDur() {
      let speed = this.speed;
      return distance => {
        return (distance / speed).toFixed(1) + 's';
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@keyframes changePosition {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(200px);
  }
}
.wave-group-1 {
  animation: changePosition 2s linear 0 infinite;
}
.wave-group-2 {
}
</style>
