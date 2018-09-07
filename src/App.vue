<template>
  <div id="app">

    <LineChart/>

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
  color: #fff;
  margin-top: 60px;
  background-color: #f47166;
}
</style>
