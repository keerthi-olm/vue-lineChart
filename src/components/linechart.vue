<!-- 

Good example here bar chart ---[ https://stackoverflow.com/questions/48726636/draw-d3-axis-without-direct-dom-manipulation 


-->

<template>
  <svg width="800" height="300">
    
    <g class='test' v-bind:transform="translate">
      <axis v-bind:scales="xAxis" v-bind:chartDefaults='chartDefaults' v-bind:data='data'/>
    <path :d="line" />
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
        margin:{top: 5, right: 50, bottom: 20, left: 50},
        data: [
   
        ]
     
    },
      line: "",
      translate:'translate(' + 50 + ',' + 5 + ')'
    };
  },
  mounted() {

    var scale={};
    var parseDate = d3.timeParse("%m-%d-%Y");

    this.data.forEach(function(d) {
      d.date = parseDate(d.day);
    });
    this.calculatePath();
  },
  methods: {

  getScales() {
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



      return { x, y };
    },

     xAxis() {
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
      // d3.axisBottom().scale(x);
      // d3.axisLeft().scale(y);
 var xaxis = d3.axisBottom()
            .scale(x)
            .tickValues(this.data.map(function(d,i){
                if(i>0) {
                    return d.date;
                } return false;
            }).splice(1))
            .ticks(4);


      return xaxis ;
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

<style lang="sass" scoped>

path
  fill: none
  stroke: #76BF8A
  stroke-width: 3px
</style>
