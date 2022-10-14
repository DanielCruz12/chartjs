<template>
  <HorizontalTestVue3 :payload="mockPayload" />
  <HorizontalTest :payload="mockPayload" />
  <Horizontal :payload="mockPayload" />
  <!-- <Vertical /> -->
</template>

<script>
import mockPayload from "./js/payload.json";

// import Vertical from "./components/Vertical.vue";
import Horizontal from "./components/Horizontal.vue";
import HorizontalTest from "./components/HorizontalTest.vue";
import HorizontalTestVue3 from "./components/HorizontalTestVue3.vue";

export default {
  name: "App",
  components: {
    // Vertical,
    Horizontal,
    HorizontalTest,
    HorizontalTestVue3,
  },
  setup() {
    return {
      mockPayload,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
