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
                      var now = new Date();
                      var res = [];
                      var len = 10;
                      while (len--) {
                          res.unshift(now.toLocaleTimeString().replace(/^\D*/,''));
                          now = new Date(now - 2000);
                      }
                      return res;
                  })()
              }
          ],
          yAxis: [
              {
                  type: 'value',
                  scale: true,
                  name: '',
                  max: 30,
                  min: 0,
                  boundaryGap: [0.2, 0.2]
              }
          ],
          series: [
              {
                  name:'最新成交价',
                  type:'line',
                  data:(function (){
                      var res = [];
                      var len = 0;
                      while (len < 10) {
                          res.push((Math.random()*10 + 5).toFixed(1) - 0);
                          len++;
                      }
                      return res;
                  })()
              }
          ]
      };
      let myChart = echarts.init(document.getElementById('main'));
      myChart.setOption(option)
      console.log(app)
      app.count = 11;
      setInterval(function (){
          console.log('setinterval')
          var axisData = (new Date()).toLocaleTimeString().replace(/^\D*/,'');

          var data0 = option.series[0].data;
          data0.shift();
          data0.push((Math.random() * 10 + 5).toFixed(1) - 0);

          option.xAxis[0].data.shift();
          option.xAxis[0].data.push(axisData);

          myChart.setOption(option);
      }, 1000);

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