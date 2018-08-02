<template>
<div id="main" style="height:500px;">
</div>
</template>
<script>
/* eslint-disable */
const echarts = require('echarts');
export default {
  data(){
    return {

    }
  },
  methods: {
    init(){
      let option = {
          title: {
              text: '',
              subtext: ''
          },
          tooltip: {
              show: false,
              trigger: 'axis',
              axisPointer: {
                  type: 'cross',
                  label: {
                      backgroundColor: '#283b56'
                  }
              }
          },
          legend: {
              data:[]
          },
          toolbox: {
              show: false,
              feature: {
                  dataView: {readOnly: false},
                  restore: {},
                  saveAsImage: {}
              }
          },
          dataZoom: {
              show: false,
              start: 0,
              end: 100
          },
          xAxis: [
              {
                  type: 'category',
                  boundaryGap: true,
                  data: (function (){
                      var res = [];
                      var len = 10;
                      while (len--) {
                          res.push(10 - len - 1);
                      }
                      console.log(res)
                      return res;
                  })()
              }
          ],
          yAxis: [
              {
                  type: 'value',
                  scale: true,
                  name: '预购量',
                  max: 1200,
                  min: 0,
                  boundaryGap: [0.2, 0.2]
              }
          ],
          series: [
              {
                  name:'预购队列',
                  type:'bar',
                  xAxisIndex: 0,
                  yAxisIndex: 0,
                  data:(function (){
                      var res = [];
                      var len = 10;
                      while (len--) {
                          res.push(Math.round(Math.random() * 1000));
                      }
                      console.log(res)                      
                      return res;
                  })()
              }
          ]
      };
      let myChart = echarts.init(document.getElementById('main'));
      myChart.setOption(option)
      console.log(app)
      app.count = 9;
      setInterval(function (){
          console.log('setinterval')
          var axisData = (new Date()).toLocaleTimeString().replace(/^\D*/,'');

          var data0 = option.series[0].data;
          // var data1 = option.series[1].data;
          data0.shift();
          data0.push(Math.round(Math.random() * 1000));
          // data1.shift();
          // data1.push((Math.random() * 10 + 5).toFixed(1) - 0);

          // option.xAxis[0].data.shift();
          // option.xAxis[0].data.push(axisData);
          option.xAxis[0].data.shift();
          option.xAxis[0].data.push(app.count++);

          myChart.setOption(option);
      }, 2100);

    }
  },
  created() {
    console.log(echarts)
    let self = this
    this.$nextTick(function(){
      self.init()
    })
  }
}
</script>