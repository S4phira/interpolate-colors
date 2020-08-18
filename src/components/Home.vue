<template>
  <div class="app">
    <div class="container">
      <div class="colorBox">
        <input type="color" class="colorBox__palette" v-model="color1" />
        <input class="colorBox__label" v-model="color1" />
      </div>
      <div class="stepsBox">
        <div class="verticalRange">
          <input type="range" :min="minSteps" :max="maxSteps" v-model="steps" />
        </div>
        <div class="steps">{{numberOfSteps}} STEPS</div>
      </div>
      <div class="colorBox">
        <input type="color" class="colorBox__palette" v-model="color2" />
        <input class="colorBox__label" v-model="color2" />
      </div>
    </div>
    <div class="colors">
      <div
        class="color"
        v-for="color in colors"
        :style="setBackgroundColor(color)"
        :key="color.name"
      ></div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      minSteps: 3,
      maxSteps: 22,
      steps: 10,
      color1: "#0ebeff",
      color2: "#ff42b3",
    };
  },
  computed: {
    numberOfSteps() {
      return this.steps - 2;
    },
    colors() {
      return this.interpolateColors(this.color1, this.color2, this.steps);
    },
  },
  methods: {
    interpolateColors(color1, color2, steps) {
      let stepFactor = 1 / (steps - 1);
      let interpolatedColorArray = [];

      color1 = this.rgbArrayFromHex(color1).map(Number);
      color2 = this.rgbArrayFromHex(color2).map(Number);

      for (var i = 0; i < steps; i++) {
        interpolatedColorArray.push(
          this.interpolateColor(color1, color2, stepFactor * i)
        );
      }
      return interpolatedColorArray;
    },
    setBackgroundColor(color) {
      return `background: rgb(${color})`;
    },
    interpolateColor(color1, color2, factor) {
      const result = color1.slice();
      for (var i = 0; i < 3; i++) {
        result[i] = Math.round(result[i] + factor * (color2[i] - color1[i]));
      }
      return result;
    },

    rgbArrayFromHex(hex) {
      const rgb = this.rgbObjectFromHex(hex);
      return [rgb.r, rgb.g, rgb.b];
    },
    rgbObjectFromHex(hex) {
      var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
      return result
        ? {
            r: parseInt(result[1], 16),
            g: parseInt(result[2], 16),
            b: parseInt(result[3], 16),
          }
        : null;
    },
  },
};
</script>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: row;
  .container {
    height: 100%;
    flex-direction: column;
    display: flex;
    align-items: center;
    justify-content: center;
    .colorBox {
      display: flex;
      margin: 10%;
      width: 300px;
      height: 100px;
      padding: 10px;
      &__palette {
        width: 30%;
        height: 100%;
      }
      &__label {
        width: 60%;
        height: 100%;
        background-color: rgb(255, 255, 255);
        margin-left: 10px;
        border-radius: 30px;
        font-size: 35px;
        display: flex;
        align-items: center;
        justify-content: center;
      }
    }
    .stepsBox {
      display: flex;
      position: relative;
      .steps {
        position: absolute;
        top: 50%;
        left: 6em;
        transform: translate(0, -50%);
        padding: 0;
        margin: 0;
        color: white;
      }
      .verticalRange {
        display: flex;
        align-items: center;
        width: 9em;
        height: 9em;
        transform: rotate(-90deg);
      }
    }
  }
  .colors {
    height: 100%;
    background-color: red;
    width: 100%;
    .color {
      padding: 0 1em;
      height: 50px;
      width: 100%;
    }
  }
}
</style>