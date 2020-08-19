<template>
  <div class="stepsBox">
    <div class="verticalRange">
      <input type="range" class="range" :min="minSteps" :max="maxSteps" v-model="steps" />
    </div>
    <div class="steps">{{numberOfSteps}} STEPS</div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      minSteps: 3,
      maxSteps: 22,
      steps: "12",
    };
  },
  computed: {
    numberOfSteps() {
      return this.steps - 2;
    },
  },
  updated() {
    this.$emit("setSteps", this.steps);
  },
};
</script>
<style lang="scss" scoped>
.stepsBox {
  display: flex;
  position: relative;
  .steps {
    position: absolute;
    top: 80%;
    left: 2em;
    transform: translate(0, -50%);
    padding: 0;
    margin: 0;
    color: white;
    font-size: 40px;
  }
  .verticalRange {
    display: flex;
    align-items: center;
    width: 19em;
    height: 9em;
    margin-left: 10px;
    input[type="range"] {
      height: 100%;
      width: 100%;
      margin: 8.35px 0;
      background-color: transparent;
      -webkit-appearance: none;
    }
    input[type="range"]:focus {
      outline: none;
    }
    input[type="range"]::-webkit-slider-runnable-track {
      background: #ffffff;
      border: 2.3px solid #ffffff;
      border-radius: 25px;
      width: 100%;
      height: 13.3px;
      cursor: pointer;
    }
    input[type="range"]::-webkit-slider-thumb {
      margin-top: -10.65px;
      width: 30px;
      height: 30px;
      background: #ff0000;
      border: 5px solid #ff0000;
      border-radius: 12px;
      cursor: pointer;
      -webkit-appearance: none;
    }
    input[type="range"]:focus::-webkit-slider-runnable-track {
      background: #ffffff;
    }

    input[type="range"]::-ms-track {
      background: transparent;
      border-color: transparent;
      border-width: 12.45px 0;
      color: transparent;
      width: 100%;
      height: 1.3px;
      cursor: pointer;
    }

    @supports (-ms-ime-align: auto) {
      input[type="range"] {
        margin: 0;
      }
    }
  }
}
</style>