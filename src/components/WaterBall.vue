<template>
  <div class="wrapper" :style="{'--size': `${size}px` }">
    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" :viewBox="`0 0 ${size} ${size}`">
      <defs>
        <clipPath id="clipPath1">
          <circle :cx="halfSize" :cy="halfSize" :r="halfSize" />
        </clipPath>
        <clipPath id="clipPath2">
          <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth - padding" />
        </clipPath>
      </defs>
      <g v-if="type === 'vertical'" clip-path="url(#clipPath1)">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth / 2" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
        <g clip-path="url(#clipPath2)">
          <path :d="verticalPathStr" stroke="transparent" :fill="waterColor">
            <animate id="animation1" attributeName="d" attributeType="XML" :values="`${verticalPathStr};${toVerticalPathStr};${verticalPathStr}`" keyTimes="0;0.5;1" begin="0s" :dur="`${dur}ms`" fill="freeze" repeatCount="indefinite" />
          </path>
        </g>
      </g>
      <g v-else clip-path="url(#clipPath1)">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth / 2" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
        <defs>
          <g id="wave">
            <path :d="horizontalPathStr" stroke="transparent" :fill="waterColor">
            </path>
          </g>
        </defs>
        <g clip-path="url(#clipPath2)">
          <g>
            <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave" :x="index * waveLength" />
            <animateTransform id="firstToleft" attributeType="XML" attributeName="transform" begin="0s" :dur="getDur(groupLength)" type="translate" fill="freeze" from="0" :to="-groupLength" />
            <animateTransform attributeType="XML" attributeName="transform" begin="firstToleft.end" :dur="getDur(2 * groupLength)" type="translate" fill="freeze" :values="`${groupLength},0;${-groupLength},0`" calcMode="linear" keyTimes="0;1" repeatCount="indefinite" />
          </g>
          <g :transform="`translate(${groupLength},0)`">
            <use v-for="(i, index) in minWaveNum" :key="i" xlink:href="#wave" :x="index * waveLength" />
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
  name: 'WaterBall',
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
    padding: {
      type: Number,
      default: 2
    },
    backgroundColor: {
      type: String,
      default: '#FFF'
    },
    waterColor: {
      type: String,
      default: '#F00'
    },
    dur: {
      // ms
      type: Number,
      default: 5000
    },
    amplitude: {
      type: Number,
      default: 0.3
    },
    waveLength: {
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
    },
    type: {
      type: String, // horizontal, vertical
      default: 'vertical'
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
    minWaveNum() {
      return Math.ceil(this.size / this.waveLength);
    },
    groupLength() {
      return this.minWaveNum * this.waveLength;
    },
    wrapperStyle() {
      const size = this.size;
      return {
        width: size,
        height: size
      };
    },
    halfSize() {
      return this.size / 2;
    },
    verticalPathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        quarterSize = Math.ceil(this.size / 4),
        amplitude = Math.floor(this.amplitude * halfSize);

      let lineY = Math.floor(size * (1 - percentage));

      let str = `m 0 ${lineY} q ${quarterSize} ${amplitude} ${halfSize} 0  t ${halfSize} 0 v ${size - lineY} h -${size} V 0`;
      return str;
    },
    toVerticalPathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        quarterSize = Math.ceil(this.size / 4),
        amplitude = Math.floor(this.amplitude * halfSize);

      let lineY = Math.floor(size * (1 - percentage));

      let str = `m 0 ${lineY} q ${quarterSize} -${amplitude} ${halfSize} 0  t ${halfSize} 0 v ${size - lineY} h -${size} V 0`;
      return str;
    },
    horizontalPathStr() {
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
      return distance => {
        return (distance / this.waveLength) * this.dur + 'ms';
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
