<template>
  <div class="colors">
    <div
      class="color"
      v-for="(color,index) in colors"
      :style="setBackgroundColor(color)"
      :key="color.name"
    >
      <div class="tooltip">
        <img
          src="../assets/copy-icon.svg"
          @click="copyColors(index)"
          :style="setContrastText(color[0],color[1],color[2],svg)"
        />
        <span
          v-if="colorModel ==='HEX'"
          class="tooltiptext"
        >Copy: {{rgbToHex(color[0],color[1],color[2])}}</span>
        <span v-else class="tooltiptext rgb">Copy: RGB: [{{color[0]}},{{color[1]}},{{color[2]}}]</span>
      </div>
      <p
        v-if="colorModel ==='HEX'"
        :id="index"
        :style="setContrastText(color[0],color[1],color[2],text)"
      >{{rgbToHex(color[0],color[1],color[2])}}</p>
      <p
        v-else
        :id="index"
        :style="setContrastText(color[0],color[1],color[2],text)"
      >R: {{color[0]}}, G: {{color[1]}}, B: {{color[2]}}</p>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    color1: String,
    color2: String,
    steps: String,
    colorModel: String,
  },
  computed: {
    colors() {
      return this.interpolateColors(this.color1, this.color2, this.steps);
    },
  },
  methods: {
    copyColors(index) {
      let copyText = document.getElementById(index).innerText;
      var inp = document.createElement("input");
      document.body.appendChild(inp);
      inp.value = copyText;
      inp.select();
      document.execCommand("copy");
      inp.remove();
    },

    setBackgroundColor(color) {
      return `background: rgb(${color})`;
    },

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

    rgbChannelToHex(channel) {
      const hex = channel.toString(16);
      return hex.length == 1 ? `0${hex}` : hex;
    },

    rgbToHex(r, g, b) {
      return `#${this.rgbChannelToHex(r)}${this.rgbChannelToHex(
        g
      )}${this.rgbChannelToHex(b)}`;
    },

    hexCopied(r, g, b) {
      let copyText = this.rgbToHex(r, g, b);
      copyText.select;
      document.execCommand("copy");
      alert("Copied: " + copyText);
    },

    setContrastText(r, g, b, type) {
      const brightness = Math.round(
        (parseInt(r) * 299 + parseInt(g) * 587 + parseInt(b) * 114) / 1000
      );
      if (type === "text")
        return brightness > 125 ? "color:black" : "color:white";
      else
        return brightness > 125
          ? "filter: grayscale(100%) brightness(0%)"
          : "filter: invert(99%) sepia(100%) saturate(29%) hue-rotate(171deg) brightness(107%) contrast(100%)";
    },
  },
};
</script>
<style lang="scss" scoped>
.colors {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px 30px;
  height: calc(95vh);
  width: 85%;
  display: flex;
  flex-direction: column;
  .color {
    padding: 0 1em;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    .tooltip {
      position: relative;
      display: inline-block;
      cursor: pointer;
    }

    .tooltip .tooltiptext {
      visibility: hidden;
      width: 120px;
      background-color: #003;
      color: #fff;
      text-align: center;
      border-radius: 6px;
      padding: 5px 0;
      position: absolute;
      z-index: 1;
      bottom: 125%;
      left: 50%;
      margin-left: -60px;
      opacity: 0;
      transition: opacity 0.3s;
    }
    .tooltip .rgb {
      width: 180px;
      margin-left: -90px;
    }

    .tooltip .tooltiptext::after {
      content: "";
      position: absolute;
      top: 100%;
      left: 50%;
      margin-left: -5px;
      border-width: 5px;
      border-style: solid;
      border-color: #000 transparent transparent transparent;
    }

    .tooltip:hover .tooltiptext {
      visibility: visible;
      opacity: 1;
    }

    img {
      width: 30px;
      margin-right: 10px;
    }
    p {
      padding-left: 30px;
      margin: 0;
      font-size: 2rem;
    }
  }
}
</style>