<template>
  <div id="app">
<svg width="500" height="270">
    <LineChart/>
  </svg>
  </div>
</template>

<script>
import LineChart from "./components/linechart";

export default {
  name: "App",
  components: {
    LineChart
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
