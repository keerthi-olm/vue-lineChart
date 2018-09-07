<template>
  <div id="app">

    <LineChart v-bind:class="[toggleClass]"/>
 <button v-on:click="play">Play again</button>

  </div>
</template>

<script>
import LineChart from "./components/linechart";

export default {
  name: "App",
  components: {
    LineChart
  },
  
  data() {
        return { 
           toggleClass:'ani1'
        } 
      },

  methods:  {

      play() { 
         // toggle classes to animate the line draw
          this.toggleClass=='ani1' ? this.toggleClass='ani2' : this.toggleClass='ani1' ;
          

      }


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
