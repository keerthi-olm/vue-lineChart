<!-- 

Good example here bar chart ---[ https://stackoverflow.com/questions/48726636/draw-d3-axis-without-direct-dom-manipulation 

-->

<template><div>
  <svg width="100%" height="100%" viewBox="0 0 800 330"
  preserveAspectRatio="xMidYMid meet">
    
    <g class='lineChart' v-bind:transform="translate">
      <axis class='yA' v-bind:scales="getScales().yAxis" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsY'/>
      <axis class='xA' v-bind:scales="getScales().xAxis" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsX()'/>
      <axis class='grid' v-bind:scales="getScales().yGrid" v-bind:chartDefaults='chartDefaults' v-bind:data='data' v-bind:trns='trnsY'/>
    <path class='line' :d="line" />
    </g>
      
  </svg>
  <p class='text' > {{chartDefaults.title}}</p>
</div>
</template>

<script>
import * as d3 from "d3";
import Axis from "./axis";
export default {
    name: "vue-line-chart",
    components: {
        axis: Axis // Using reusable component to draw x,y axis and Grid.
    },
    data() {
        return {
            data: [{
                day: "02-11-2016",
                count: 80
            }, {
                day: "02-12-2016",
                count: 250
            }, {
                day: "02-13-2016",
                count: 150
            }, {
                day: "02-14-2016",
                count: 496
            }, {
                day: "02-15-2016",
                count: 140
            }, {
                day: "02-16-2016",
                count: 380
            }, {
                day: "02-17-2016",
                count: 100
            }, {
                day: "02-18-2016",
                count: 150
            }],
            chartDefaults: {

                width: 800,
                height: 300,
                chartId: 'linechart-vue',
                title: 'UK Rainfall for 2018',
                margin: {
                    top: 5,
                    right: 5,
                    bottom: 15,
                    left: 50
                },
                data: [

                ]

            },
            line: "",
            //Translate co-ords for chart, x axis and yaxis. This is injected into template
            translate: 'translate(' + 50 + ',' + 5 + ')',
            trnsY: 'translate(0,0)',
            trnsX: this.getTrnsx
        };
    },
    mounted() {

        var scale = {};
        //Kick off drawing chart once component is mounted
        this.calculatePath();
      
    },
    methods: {

        getScales() {
          // All the maths to work chart co ordinates and woring out Axis
                var parseDate = d3.timeParse("%m-%d-%Y");

                this.data.forEach(function(d) {
                    d.date = parseDate(d.day);
                });

                const x = d3.scaleTime()
                    .domain(d3.extent(this.data, function(d) {
                        return d.date;
                    }))
                    .rangeRound([0, this.chartDefaults.width]);
                const y = d3.scaleLinear()
                    .domain([0, d3.max(this.data, function(d) {
                        return d.count + 100;
                    })])
                    .range([this.chartDefaults.height, 0]);
                d3.axisBottom().scale(x);
                d3.axisLeft().scale(y);
                
                //Key funtions to draw X-axis,YAxis and the grid. All uses component axis 
                var xAxis = d3.axisBottom()
                    .scale(x).tickFormat(d3.timeFormat("%b-%d"))
                    .tickValues(this.data.map(function(d, i) {
                        if (i > 0) {
                            return d.date;
                        }
                        return false;
                    }).splice(1))
                    .ticks(4);

                var yAxis = d3.axisLeft()
                    .scale(y)
                    .ticks(5);
                var yGrid = d3.axisLeft()
                    .scale(y)
                    .tickSize(-(this.chartDefaults.width), 0, 0)
                    .tickFormat("");
                 // Return the key calculations and functions to draw the chart  
                return {
                    x, y, xAxis, yAxis, yGrid
                };
            },
        getTrnsx(chartDefaults) { //works out translate value in realtive to dynamic height 
                const t = "translate(0," + (this.chartDefaults.height) + ")";
                return t
            },
        calculatePath() {
                
                //Get key calculation funtions to draw chart , Ie scale, axis mapping and plotting
                const scale = this.getScales();
                 // Define calcultion to draw chart
                const path = d3
                    .line()
                    .x((d) => {
                        return scale.x(d.date)
                    })
                    .y(d => {;
                        return scale.y(d.count)
                    }).curve(d3.curveCardinal);

                 // draw line then this.line is injected into the template   
                this.line = path(this.data);

            }
    }
};
</script>
<!-- css loaderhttps://vue-loader.vuejs.org/guide/scoped-css.html#mixing-local-and-global-styles -->
<style>

text {color: #fff;}

path.line  {fill: none;
  stroke: #ecbc3a;
  stroke-width: 3px;
}

.grid line {opacity: 0.05;}
.xA line {opacity: 0.5;}

/*Some fancy animation to draw chart*/
svg .lineChart>path {
  stroke: #ecbc3a;
 
  stroke-width: 3;
  stroke-dasharray: 4813.713;
  stroke-dashoffset: 4813.713;
  animation-name: draw;
  animation-duration: 10s;
  animation-fill-mode: forwards;
  animation-iteration-count: 1;
  animation-timing-function: linear;
}

#Layer_1 {
  width: 100%;
}
@keyframes draw {
  85% {
   
  }
  100% {
    stroke-dashoffset: 0;
    
  }
}
.text {
  display: inline-block;
  font-size:3vw;
  margin: 0.5vw 0 1.5vw;
}

svg {
  background-color: #f47166;
}
</style>
