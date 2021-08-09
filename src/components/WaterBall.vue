<template>
  <svg version="1.1" xmlns="http://www.w3.org/2000/svg" :width="size" :height="size" :viewBox="`0 0 ${size} ${size}`">
    <defs>
      <clipPath id="clipPath">
        <circle :cx="halfSize" :cy="halfSize" :r="halfSize" />
      </clipPath>
    </defs>
    <g clip-path="url(#clipPath)">
      <circle :cx="halfSize" :cy="halfSize" :r="halfSize" :stroke-width="borderWidth" :stroke="borderColor" :fill="backgroundColor" />
      <g>
        <path :d="pathStr" :stroke="waterColor" :fill="waterColor">
        </path>
        <animateTransform attributeName="transform" attributeType="XML" type="translate" from="0" :to="-size" :dur="duration" repeatCount="indefinite">
        </animateTransform>
      </g>
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
    duration: {
      type: String,
      default: '1.5s'
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
      return Math.ceil(this.size / (this.waveLength / 2)) + 1;
    },
    pathStr() {
      const size = this.size,
        percentage = this.percentage,
        halfSize = this.halfSize,
        amplitude = Math.floor(this.amplitude * halfSize),
        waveLength = this.waveLength,
        minWaveNum = this.minWaveNum,
        halfWaveLength = Math.floor(waveLength / 2),
        quarterWaveLength = Math.floor(waveLength / 4);

      let lineY = Math.floor(size * (1 - percentage));
      let arr = [];

      if (minWaveNum < amplitude.length) {
        arr = arr.concat(amplitude);
      } else {
        let loopNum = Math.ceil(minWaveNum / amplitude.length);
        if (!(minWaveNum % amplitude.length)) {
          loopNum++;
        }
        // for (let i = 0; i < loopNum; i++) {
        //   arr = arr.concat(amplitude);
        // }

        arr = arr.concat(...Array(loopNum).fill(amplitude));
      }

      if (arr.length % 2) {
        arr.push(amplitude[0]);
      }

      let str = `m 0 ${lineY} ${arr
        .map((num, index) => {
          console.log(num);
          if (index % 2) {
            return `q ${quarterWaveLength} ${num} ${halfWaveLength} 0 `;
          } else {
            return `q ${quarterWaveLength} -${num} ${halfWaveLength} 0 `;
          }
        })
        .join(' ')} v ${size - lineY} h -${arr.length * halfWaveLength} V`;

      // if (minWaveNum < amplitude.length) {
      //   for (let i = 0, len = amplitude.length; i < len; i++) {
      //     // str += `q ${quarterWaveLength} -${amplitude[i]} ${quarterWaveLength * 2} 0  t ${quarterWaveLength * 2} 0 `;
      //     if (i % 2) {
      //       str += `q ${quarterWaveLength} -${amplitude[i]} ${quarterWaveLength * 2} 0  t ${quarterWaveLength * 2} 0 `;
      //     } else {
      //       str += `q ${quarterWaveLength} -${amplitude[i]} ${quarterWaveLength * 2} 0  t ${quarterWaveLength * 2} 0 `;
      //     }
      //   }

      //   str += `v ${size - lineY} h -${amplitude.length * waveLength} V`;
      // }

      // console.log(str);

      // return 'm 0 100 q 50 -50 100 0 t 100 0 q 50 -50 100 0 t 100 0 v 100 h -400 V';
      console.log(str);
      return str;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
