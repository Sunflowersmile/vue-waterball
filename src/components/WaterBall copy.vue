<template>
  <div class="wrapper" :style="wrapperStyles">
    <div class="water" :style="waterStyles"></div>
  </div>
</template>

<script>
export default {
  name: "WaterBall",
  props: {
    size: {
      type: [Number, String],
      default: 200,
    },
    percentage: {
      type: [Number, String],
      default: 50,
    },
    borderWidth: {
      type: [Number, String],
      default: 3,
    },
    borderColor: {
      type: String,
      default: "red",
    },
    backgroundColor: {
      type: String,
      default: "#FFF",
    },
    waterColor: {
      type: String,
      default: "#F00",
    },
    amplitude: {
      // 振幅
      type: [Number, String],
      default: 20,
    },
    waveLength: {
      // 波长
      type: [Number, String],
      default: 120,
    },
  },
  computed: {
    wrapperStyles() {
      let size = this.size,
        borderWidth = this.borderWidth,
        borderColor = this.borderColor,
        backgroundColor = this.backgroundColor;
      return {
        width: size + "px",
        height: size + "px",
        borderWidth: borderWidth + "px",
        borderColor,
        backgroundColor,
      };
    },
    waterStyles() {
      let overlay = 5;
      return {
        "--waterColor": this.waterColor,
        "--amplitude": this.amplitude,
        "--waveLength": this.waveLength,
        "--waveHeight": this.amplitude * 2 + "px",
        "--ellipseRadiusX": this.waveLength / 4 + "px",
        "--ellipseRadiusY": this.amplitude + "px",
        "--backgroundSizeX": this.waveLength + "px",
        "--backgroundSizeY": this.amplitude + "px",
        "--backgroundPositionX": this.waveLength / 2 + "px",
        "--currentY": this.amplitude - overlay + "px",
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
  border-radius: 50%;
  border-style: solid;
  position: relative;
  box-sizing: border-box;
  overflow: hidden;
}

.water {
  width: 200px;
  height: 100px;
  background-color: var(--waterColor);
  position: absolute;
  top: 100px;
}

.water:before {
  content: "";
  position: absolute;
  height: var(--waveHeight);
  left: 0;
  right: 0;
  top: -39px;
  background: radial-gradient(
        var(--ellipseRadiusX) var(--ellipseRadiusY) ellipse at
          var(--ellipseRadiusX) -5px,
        transparent var(--ellipseRadiusX) var(--ellipseRadiusY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY)
      )
      repeat-x,
    radial-gradient(
        var(--ellipseRadiusX) var(--ellipseRadiusY) ellipse at
          var(--ellipseRadiusX) var(--currentY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY),
        red var(--ellipseRadiusX) var(--ellipseRadiusY),
        transparent var(--ellipseRadiusX) var(--ellipseRadiusY)
      )
      repeat-x;
  background-size: var(--backgroundSizeX) var(--backgroundSizeY);
  /* background-position: -40px calc(100% + 62px), 0 calc(100% - 18px); */
  background-position: 0 var(--currentY), var(--backgroundPositionX) 5px;
}
</style>
