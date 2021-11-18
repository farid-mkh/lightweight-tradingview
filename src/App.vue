<template>
  <div class="row">
    <div class="col-6" style="height: 300px">
      <LightweightTradingview />
    </div>
  </div>
</template>

<script>
import LightweightTradingview from "./components/LightweightTradingview.vue";

export default {
  name: "App",
  components: {
    LightweightTradingview,
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
