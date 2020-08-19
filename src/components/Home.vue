<template>
  <div class="app">
    <div class="container">
      <div>
        <button class="btn" @click="changeColorModel">{{colorModel}}</button>
      </div>
      <div class="colorBox">
        <input type="color" class="colorBox__palette" v-model="color1" />
        <input class="colorBox__label" v-model="color1" />
      </div>
      <button
        :style="{ color: color1, borderColor: color1 }"
        class="btn"
        @click="randomHexColor(1)"
      >random</button>
      <Range @setSteps="setSteps($event)" />
      <div class="colorBox">
        <input type="color" class="colorBox__palette" v-model="color2" />
        <input class="colorBox__label" v-model="color2" />
      </div>
      <button
        :style="{ color: color2, borderColor: color2 }"
        class="btn"
        @click="randomHexColor(2)"
      >random</button>
    </div>
    <Colors :color1="color1" :color2="color2" :steps="steps" :colorModel="colorModel" />
  </div>
</template>
<script>
import Range from "./Range.vue";
import Colors from "./Colors.vue";
export default {
  components: {
    Range,
    Colors,
  },
  data() {
    return {
      color1: "#d03991",
      color2: "#3246a9",
      steps: "12",
      colorModel: "HEX",
    };
  },
  methods: {
    changeColorModel() {
      this.colorModel == "HEX"
        ? (this.colorModel = "RBG")
        : (this.colorModel = "HEX");
    },
    setSteps(value) {
      this.steps = value;
    },
    randomHexColor(color) {
      const randomColor =
        "#" +
        ("00" + Math.floor(Math.random() * 16777216).toString(16)).substr(-6);
      if (color === 1) {
        this.color1 = randomColor;
      } else {
        this.color2 = randomColor;
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: row;
  align-items: center;
  .container {
    height: 100%;
    flex-direction: column;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 20px;
    .btn {
      background-color: transparent;
      border-radius: 32px;
      border: 1px solid #ffffff;
      display: inline-block;
      cursor: pointer;
      font-size: 18px;
      padding: 6px 13px;
      text-decoration: none;
      color: white;
      outline: none;
    }

    .colorBox {
      display: flex;
      margin: 10%;
      width: 300px;
      height: 70px;
      padding: 10px;
      justify-content: center;
      input {
        border: none;
        outline: none;
        border-radius: 0 1em 1em 0;
        &[type="color"] {
          padding: 0;
          width: 70px;
          height: 102%;
          border-radius: 30px 0 0 30px;
          background-color: white;
        }
        &::-moz-color-swatch {
          border: none;
          border-radius: 4em;
        }

        &::-webkit-color-swatch {
          border: none;
          border-radius: 4em;
        }
        &::-webkit-color-swatch-wrapper {
          padding: 0.35em;
          border-radius: 4em;
        }
      }

      &__label {
        padding-left: 15px;
        width: 60%;
        height: 100%;
        background-color: rgb(255, 255, 255);
        border-radius: 0 30px 30px 0;
        font-size: 35px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: "Pangolin", cursive;
      }
    }
  }
}
</style>