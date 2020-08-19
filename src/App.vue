<template>
  <div id="app">
    <Home />
  </div>
</template>

<script>
import Home from "./components/Home.vue";
export default {
  name: "App",
  components: { Home },
};
</script>

<style>
body {
  margin: 0;
}
#app {
  background-color: black;
  font-family: "Pangolin", cursive;
  width: calc(100vw);
  height: calc(100vh);
}
</style>
