<template>
  <div id="svg-wrapper">
  </div>
</template>

<script>
import * as d3 from "d3"
export default {
  name: 'HelloWorld',
  data() {
    return {

    }
  },
  created() {
    /* eslint-disable */
    console.log(d3)
    this.init()
  },
  methods: {
    init() {
      console.log(d3)
      this.$nextTick(_ => {
        var svg = null;
        var circle = null;
        // var circleTransition = null;
        var latestBeat = null;
        var insideBeat = false;
        var data = [];

        var SECONDS_SAMPLE = 5;
        var BEAT_TIME = 400;
        var TICK_FREQUENCY = SECONDS_SAMPLE * 1000 / BEAT_TIME;
        var BEAT_VALUES = [0, 0, 3, -4, 10, -7, 3, 0, 0];
        var BEAT_2_VALUES = [0, 0, 3, -4, 10, -4, 3, 0, 0];
        var BEAT_2_VALUES = [0, 0, 0, 0, 0, 0, 0, 0, 0];
        console.log('BEAT_VALUES')
        console.log(BEAT_VALUES)
        var CIRCLE_FULL_RADIUS = 40;
        var MAX_LATENCY = 5000;

        function beat() {

          if (insideBeat) return;
          insideBeat = true;

          var now = new Date();
          var nowTime = now.getTime();

          if (data.length > 0 && data[data.length - 1].date > now) {
            data.splice(data.length - 1, 1);
          }

          data.push({
            date: now,
            value: 0
          });

          var step = BEAT_TIME / BEAT_VALUES.length - 2;
          let boolean = Math.random().toFixed()==='0'
          for (var i = 1; i < BEAT_VALUES.length; i++) {
            data.push({
              date: new Date(nowTime + i * step),
              value: boolean?BEAT_VALUES[i]:BEAT_2_VALUES[i]
            });
          }

          // var step = BEAT_TIME / BEAT_2_VALUES.length - 2;
          // for (var i = 1; i < BEAT_2_VALUES.length; i++) {
          //   data.push({
          //     date: new Date(nowTime + i * step),
          //     value: BEAT_2_VALUES[i]
          //   });
          // }

          latestBeat = now;
          /*
          circleTransition = circle.transition()
            .duration(BEAT_TIME)
            .attr("r", CIRCLE_FULL_RADIUS)
            .attr("fill", "#6D9521");
          */
          setTimeout(function () {
            insideBeat = false;
          }, BEAT_TIME);
        }
      
        var svgWrapper = document.getElementById("svg-wrapper");
        var margin = { left: 10, top: 10, right: CIRCLE_FULL_RADIUS * 3, bottom: 10 },
          width = svgWrapper.offsetWidth - margin.left - margin.right,
          height = svgWrapper.offsetHeight - margin.top - margin.bottom;
          console.log(`height is: ${height}`)
          // height = 500
        // create SVG
        svg = d3.select('#svg-wrapper').append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.bottom + margin.top)
          .append("g")
          .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        // init scales
        var now = new Date(),
          fromDate = new Date(now.getTime() - SECONDS_SAMPLE * 1000);

        // create initial set of data
        data.push({
          date: now,
          value: 0
        });

        var x = d3.scaleTime()
          .domain([fromDate, new Date(now.getTime())])
          .range([0, width]),
          y = d3.scaleLinear()
            .domain([-10, 10])
            .range([height, 0]);
        console.log('y')
        console.log(y)
        console.log('d3.curveBasis')
        console.log(d3.curveBasis)
        var line = d3.line()
          .curve(d3.curveBasis)
          // .interpolate("basis")
          .x(function (d) {
            // console.log(d)
            return x(d.date);
          })
          .y(function (d) {
            // debugger
            // console.log(d.value)
            return y(d.value);
          });
        var xAxis = d3.axisBottom(x)
        // var xAxis = d3.svg.axis()
          // .scale(x)
          // .orient("bottom")
          .tickArguments(d3.timeSeconds, 1)
          .tickFormat(function (d) {
            var seconds = d.getSeconds() === 0 ? "00" : d.getSeconds();
            return seconds % 10 === 0 ? d.getMinutes() + ":" + seconds : ":" + seconds;
          });

        // add clipPath
        svg.append("defs").append("clipPath")
          .attr("id", "clip")
          .append("rect")
          .attr("width", width)
          .attr("height", height);

        var axis = d3.select("svg").append("g")
          .attr("class", "axis")
          .attr("transform", "translate(0," + height + ")")
          .call(xAxis);

        var path = svg.append("g")
          .attr("clip-path", "url(#clip)")
          .append("path")
          .attr("class", "line");
        console.log(svg.select(".line"))
        svg.select(".line")
          .attr("d", line(data));
        console.log(`path:${d3.select("path")}`)
        let t = d3.select("path")
        debugger
        console.log(t)
        var transition = d3.select("path").transition()
          .duration(100)
          .ease(d3.easeLinear);

        (function tick() {

          transition = transition.each(function () {

            // update the domains
            now = new Date();
            fromDate = new Date(now.getTime() - SECONDS_SAMPLE * 1000);
            x.domain([fromDate, new Date(now.getTime() - 100)]);

            var translateTo = x(new Date(fromDate.getTime()) - 100);

            // redraw the line
            svg.select(".line")
              .attr("d", line(data))
              .attr("transform", null)
              .transition()
              .attr("transform", "translate(" + translateTo + ")");

            // slide the x-axis left
            axis.call(xAxis);

          }).transition().on("start", tick);
        })();
        console.log('TICK_FREQUENCY')
        console.log(TICK_FREQUENCY)
        // requestAnimationFrame(function () {
        //   // now = new Date();
        //   // fromDate = new Date(now.getTime() - SECONDS_SAMPLE * 1000);

        //   // for (var i = 0; i < data.length; i++) {
        //   //   if (data[i].date < fromDate) {
        //   //     data.shift();
        //   //   } else {
        //   //     break;
        //   //   }
        //   // }
        //   // console.log(insideBeat)
        //   if (insideBeat) return;
        //       data.shift();

        //   data.push({
        //     // date: now,
        //     value: 0
        //   });
        // })
        function si () {
          now = new Date();
          fromDate = new Date(now.getTime() - SECONDS_SAMPLE * 1000);

          for (var i = 0; i < data.length; i++) {
            if (data[i].date < fromDate) {
              data.shift();
            } else {
              break;
            }
          }
          setTimeout(si,TICK_FREQUENCY)
          // console.log(insideBeat)
          if (insideBeat) return;

          data.push({
            date: now,
            value: 0
          });
        }
        setTimeout(si,TICK_FREQUENCY)
        // setInterval(function () {
        //   now = new Date();
        //   fromDate = new Date(now.getTime() - SECONDS_SAMPLE * 1000);

        //   for (var i = 0; i < data.length; i++) {
        //     if (data[i].date < fromDate) {
        //       data.shift();
        //     } else {
        //       break;
        //     }
        //   }
        //   // console.log(insideBeat)
        //   if (insideBeat) return;

        //   data.push({
        //     date: now,
        //     value: 0
        //   });
        // }, TICK_FREQUENCY);

        setInterval(function () {
          beat();
        }, 2000);
        beat();
        })

    
    }
  }
}
</script>

  <style>
    #svg-wrapper {
      width: 500px;
      height: 160px;
      margin: 2em auto;
    }

    svg path {
      fill: none;
      stroke: #000;
      stroke-width: 1.5px;
    }

    svg .axis {
      font-size: 12px;
    }

    svg .axis path {
      display: none;
    }
  </style>
