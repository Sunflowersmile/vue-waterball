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
      <g clip-path="url(#clipPath1)">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize - borderWidth / 2" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
        <g id="wave1" clip-path="url(#clipPath2)">
          <path :d="pathStr" stroke="transparent" :fill="waterColor">
            <animate id="animation1" attributeName="d" attributeType="XML" :values="`${pathStr};${toPathStr};${pathStr}`" keyTimes="0;0.5;1" begin="0s" :dur="dur" fill="freeze" repeatCount="indefinite" />
          </path>
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
  name: 'WaterBall1',
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
      type: String,
      default: '5s'
    },
    amplitude: {
      // 振幅
      type: Number,
      default: 0.3
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
    pathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        quarterSize = Math.ceil(this.size / 4),
        amplitude = Math.floor(this.amplitude * halfSize);

      let lineY = Math.floor(size * (1 - percentage));

      let str = `m 0 ${lineY} q ${quarterSize} ${amplitude} ${halfSize} 0  t ${halfSize} 0 v ${size - lineY} h -${size} V 0`;
      return str;
    },
    toPathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        quarterSize = Math.ceil(this.size / 4),
        amplitude = Math.floor(this.amplitude * halfSize);

      let lineY = Math.floor(size * (1 - percentage));

      let str = `m 0 ${lineY} q ${quarterSize} -${amplitude} ${halfSize} 0  t ${halfSize} 0 v ${size - lineY} h -${size} V 0`;
      return str;
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
