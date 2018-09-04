<template>
  
                <g ref="axis" :transform='translate' >
                </g>

</template>

<script>

import * as d3 from "d3";
export default {
  name: "axis",
  props: {
   scales:Function,
   chartDefaults:Object
  },

  data() {
    return {translate :"translate(0,"+(this.chartDefaults.height - (this.chartDefaults.margin.top + this.chartDefaults.margin.bottom))+")",
    width:0,
    height:0
}
},
  mounted() {
 console.log('--->');
  	var node=this.$refs.axis;
    // var xScale=this.scales();
console.log(this.scales());
 d3.select(node).call(this.scales());

  }
}
</script>

