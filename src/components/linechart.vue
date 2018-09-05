<!-- 

Good example here bar chart ---[ https://stackoverflow.com/questions/48726636/draw-d3-axis-without-direct-dom-manipulation 


-->

<template>
  <svg width="100%" height="100%" viewBox="0 0 800 400"
  preserveAspectRatio="xMidYMid meet">
    
    <g class='test' v-bind:transform="translate">
      <axis class='yA' v-bind:scales="getScales().yAxis" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsY'/>
      <axis class='xA' v-bind:scales="getScales().xAxis" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsX()'/>
      <axis class='grid' v-bind:scales="getScales().yGrid" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsY'/>
    <path class='line' :d="line" />
    </g>
      
  </svg>
</template>

<script>
import * as d3 from "d3";
import Axis from "./axis";
export default {
  name: "vue-line-chart",  
  components: {
    axis:Axis
  },
  data() {
    return {
      data: [
        { day: "02-11-2016", count: 80 },
        { day: "02-12-2016", count: 250 },
        { day: "02-13-2016", count: 150 },
        { day: "02-14-2016", count: 496 },
        { day: "02-15-2016", count: 140 },
        { day: "02-16-2016", count: 380 },
        { day: "02-17-2016", count: 100 },
        { day: "02-18-2016", count: 150 }
      ],
      chartDefaults: {
    
        width: 800,
        height: 300,
        chartId: 'linechart-redux',
        margin:{top: 5, right: 50, bottom: 15, left: 50},
        data: [
   
        ]
     
    },
      line: "",
      translate:'translate(' + 50 + ',' + 5 + ')',
      trnsY:'translate(0,0)',
      trnsX: this.getTrnsx
    };
  },
  mounted() {
console.log(this.data);
    var scale={};

    this.calculatePath();
  },
  methods: {

  getScales() {
          var parseDate = d3.timeParse("%m-%d-%Y");

    this.data.forEach(function(d) {
      d.date = parseDate(d.day);
    });

        const x =  d3.scaleTime()
            .domain(d3.extent(this.data, function (d) {
                return d.date;
            }))
            .rangeRound([0, this.chartDefaults.width]);
        const y = d3.scaleLinear()
            .domain([0,d3.max(this.data,function(d){
                return d.count+100;
            })])
            .range([this.chartDefaults.height, 0]);
      d3.axisBottom().scale(x);
      d3.axisLeft().scale(y);

        var xAxis = d3.axisBottom()
            .scale(x).tickFormat(d3.timeFormat("%b-%d"))
            .tickValues(this.data.map(function(d,i){
                if(i>0) {
                    return d.date;
                } return false;
            }).splice(1))
            .ticks(4);

        var yAxis = d3.axisLeft()
            .scale(y)
            .ticks(5);
      var yGrid = d3.axisLeft()
            .scale(y)
            .tickSize(-(this.chartDefaults.width), 0, 0)
            .tickFormat("");

      return { x, y, xAxis,yAxis,yGrid };
    },
getTrnsx(chartDefaults) {
  const t="translate(0,"+(this.chartDefaults.height )+")";
  return t
},
    calculatePath() {
    
      const scale = this.getScales();
        
      const path = d3
        .line()
        .x((d) => {  return scale.x(d.date)})
        .y(d => {;return scale.y(d.count)}).curve(d3.curveCardinal);;
      this.line = path(this.data);
      
    }
  }
};
</script>
<!-- css loaderhttps://vue-loader.vuejs.org/guide/scoped-css.html#mixing-local-and-global-styles -->
<style>


path.line  {fill: none;
  stroke: blue;
  stroke-width: 3px;
}
</style>
