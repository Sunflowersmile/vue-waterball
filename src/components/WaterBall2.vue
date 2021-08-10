<template>
  <div class="wrapper" :style="{'--size': `${size}px` }">
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" :viewBox="`0 0 ${size} ${size}`">
      <defs>
        <clipPath id="clipPath3">
          <circle :cx="halfSize" :cy="halfSize" :r="halfSize" />
        </clipPath>
        <clipPath id="clipPath4">
          <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth - padding" />
        </clipPath>
      </defs>
      <g clip-path="url(#clipPath3)">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth / 2" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
        <defs>
          <g id="wave2">
            <path :d="pathStr" stroke="transparent" :fill="waterColor">
            </path>
          </g>
        </defs>
        <g clip-path="url(#clipPath4)">
          <g class="wave-group-1">
            <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave2" :x="index * waveLength" />
            <animateTransform id="firstToleft" attributeType="XML" attributeName="transform" begin="0s" :dur="getDur(groupLength)" type="translate" fill="freeze" from="0" :to="-groupLength" />
            <animateTransform attributeType="XML" attributeName="transform" begin="firstToleft.end" :dur="getDur(2 * groupLength)" type="translate" fill="freeze" :values="`${groupLength},0;${-groupLength},0`" calcMode="linear" keyTimes="0;1" repeatCount="indefinite" />
          </g>
          <g class="wave-group-2" :transform="`translate(${groupLength},0)`">
            <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave2" :x="index * waveLength" />
            <animateTransform attributeType="XML" attributeName="transform" begin="0s" :dur="getDur(2 * groupLength)" type="translate" fill="freeze" :values="`${groupLength},0;${-groupLength},0`" calcMode="linear" keyTimes="0;1" repeatCount="indefinite" />
          </g>
        </g>
      </g>
    </svg>
    <div class="cover">
      <slot>
        <span class="cover-default" :style="{'--fontSize': `${fontSize}px`, '--color': fontColor }">{{percentage | toPercentage}}</span>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WaterBall2',
  props: {
    size: {
      type: Number,
      default: 200
    },
    percentage: {
      type: Number,
      default: 0.5
    },
    borderWidth: {
      type: Number,
      default: 5
    },
    borderColor: {
      type: String,
      default: '#F00'
    },
    backgroundColor: {
      type: String,
      default: '#FFF'
    },
    waterColor: {
      type: String,
      default: '#F00'
    },
    padding: {
      type: Number,
      default: 2
    },
    speed: {
      // px/s
      type: Number,
      default: 50
    },
    amplitude: {
      // 振幅
      type: Number,
      default: 0.3
    },
    waveLength: {
      // 波长
      type: Number,
      default: 200
    },
    fontSize: {
      type: Number,
      default: 20
    },
    fontColor: {
      type: String,
      default: '#000'
    }
  },
  filters: {
    toPercentage(percentage) {
      if (percentage && percentage > 0) {
        return Number((percentage * 100).toFixed(2)) + '%';
      }
      return '';
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

      let str = `m 0 ${lineY} q ${quarterWaveLength} -${amplitude} ${halfWaveLength} 0  t ${halfWaveLength} 0 v ${size - lineY} h -${waveLength} V 0`;
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
<style scoped>
.wrapper {
  width: var(--size);
  height: var(--size);
  position: relative;
}
.cover {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
.cover-default {
  position: absolute;
  top: 50%;
  height: 80px;
  line-height: 80px;
  margin-top: -40px;
  left: 50%;
  transform: translateX(-50%);
  font-size: var(--fontSize);
  color: var(--color);
}
</style>