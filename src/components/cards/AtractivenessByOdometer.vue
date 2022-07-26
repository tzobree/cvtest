<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Atractiveness by odometer</h4>
        <span class="tooltip">icon</span>
      </div>
      <div class="right">
        <span>arrow</span>
      </div>
    </div>
    <div class="card-content">
      
      <HighChart
      :options="chartOptions" ref="chartRef"
      class="highcharts">
      </HighChart>

    </div>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue'
import Highcharts from 'highcharts'
import more from 'highcharts/highcharts-more'

more(Highcharts)


export default {
  components: {
    HighChart: Chart,
  },
  props: {
    chartData: {
      type: Array,
      default () {
        return  []
      }
    },
    thisCar: {
      type: Number,
      default () {
        return 0
      }
    }
  },
  data () {
    return {
      chartOptions: {
        chart: {
          height: 200
        },
        title: null,
        legend: {
          enabled: false
        },
        credits: {
          enabled: false
        },
        xAxis: [
          {
            title: { 
              enabled: false
            },
            alignTicks: false,
            // tickInterval: 50000,
            tickLength: 0,
            lineColor: '#062931',
            gridLineDashStyle: 'solid',
            gridLineWidth: 0.5,
            gridLineColor: 'rgba(6, 41, 49, 0.1)',
            labels: {
              style: {
                color: 'rgba(6, 41, 49, 0.4)',
                fontSize: 13,
                fontFamily: 'DM Sans'
              },
              formatter:function(){
                if(this.value != 0){
                  return this.value/1000 + 'k'
                }
              }
            }
          }
        ],
        yAxis: [
          {
            title: { 
              text: 'Atractiveness',
              style: {
                color: 'rgba(6, 41, 49, 0.4)',
                fontSize: 13,
                fontFamily: 'DM Sans'
              }
            },
            gridLineDashStyle: 'solid',
            gridLineWidth: 0.5,
            gridLineColor: 'rgba(6, 41, 49, 0.1)',
            labels: {
              enabled: false
            },
            // min: 0
          }
        ],
        plotOptions: {
          histogram: {
            accessibility: {
              point: {
                valueDescriptionFormat: '{index}. {point.x:.3f} to {point.x2:.3f}, {point.y}.'
              }
            }
          }
        },
        series: [
          // {
          //   name: 'Histogram',
          //   type: 'histogram',
          //   xAxis: 1,
          //   yAxis: 1,
          //   baseSeries: 's1',
          //   zIndex: -1
          // },
          {
            name: '',
            type: 'spline',
            color: 'black',
            data: [],
            // data: [
            //   [ 100, 2.21827619738121e-06],
            //   [10302.0408163265, 2.75822881415259e-06],
            //   [20504.0816326531, 3.29200158045949e-06],
            //   [30706.1224489796, 3.78055205466270e-06],
            //   [40908.1632653061, 4.18940279600193e-06],
            //   [51110.2040816327, 4.49457122891701e-06],
            //   [61312.2448979592, 4.68624198739140e-06],
            //   [71514.2857142857,	4.76895061006053e-06],
            //   [81716.3265306123,	4.75855413969930e-06],
            //   [91918.3673469388,	4.67722756157302e-06],
            //   [102120.408163265,	4.54788260854894e-06],
            //   [112322.448979592,	4.38969461676134e-06],
            //   [122524.489795918,	4.21550819478000e-06],
            //   [132726.530612245,	4.03147046380484e-06],
            //   [142928.571428571,	3.83836122676724e-06],
            //   [153130.612244898,	3.63393686395325e-06],
            //   [163332.653061225,	3.41534311037152e-06],
            //   [173534.693877551,	3.18107788899919e-06],
            //   [183736.734693878,	2.93199355338864e-06],
            //   [193938.775510204,	2.67155790057701e-06],
            //   [204140.816326531,	2.40523314209339e-06],
            //   [214342.857142857,	2.13965080783434e-06],
            //   [224544.897959184,	1.88145861363964e-06],
            //   [234746.938775510,	1.63662561542389e-06],
            //   [244948.979591837,	1.40971423372741e-06],
            //   [255151.020408163,	1.20360359717302e-06],
            //   [265353.061224490,	1.01956321102821e-06],
            //   [275555.102040816,	8.57461586781104e-07],
            //   [285757.142857143,	7.16089947515268e-07],
            //   [295959.183673469,	5.93643993858595e-07],
            //   [306161.224489796,	4.88133632256805e-07],
            //   [316363.265306122,	3.97677929555442e-07],
            //   [326565.306122449,	3.20717338873791e-07],
            //   [336767.346938776,	2.55875835468102e-07],
            //   [346969.387755102,	2.01939299257504e-07],
            //   [357171.428571429,	1.57775505611281e-07],
            //   [367373.469387755,	1.22176536340575e-07],
            //   [377575.510204082,	9.39947932833300e-08],
            //   [387777.551020408,	7.20105429216218e-08],
            //   [397979.591836735,	5.50800444392071e-08],
            //   [408181.632653061,	4.21973764855983e-08],
            //   [418383.673469388,	3.24509686378852e-08],
            //   [428585.714285714,	2.50772445612200e-08],
            //   [438787.755102041,	1.94504949557164e-08],
            //   [448989.795918367,	1.50720754461229e-08],
            //   [459191.836734694,	1.16051316931686e-08],
            //   [469393.877551020,	8.81208794828731e-09],
            //   [479595.918367347,	6.54948149076105e-09],
            //   [489797.959183674,	4.72625230484480e-09],
            //   [500000,	3.29448141288146e-09]
            // ],
            id: 's1',
            marker: {
              radius: 0
            },
            lineWidth: 1.5
          }
        ]
      }
    }
  },
  mounted () {
    let chart = this.$refs.chartRef.chart
    // console.log('chart', chart)
    let calcData = this.chartData
    chart.update({
      series: {
        data: calcData
      }
    })
    this.drawElements()
  },
  methods: {
    drawElements () {
      const chart = this.$refs.chartRef.chart
      let xAxis = chart.xAxis[0]
      let yAxis = chart.yAxis[0]
      let yStart = yAxis.toPixels(0)


      let thisCarXValue = this.thisCar
      let thisCarXCalc = xAxis.toPixels(thisCarXValue)
      // let thisCarYValue = 30
      // let thisCarYCalc = 160 - thisCarYValue * 3
      let thisCarYCalc = 20


      chart.renderer.path(['M', thisCarXCalc, yStart, 'L', thisCarXCalc, thisCarYCalc])
        .attr({
          'stroke-width': 2,
          stroke: '#FFC063',
          zIndex: 5
        })
        .add()
      
      chart.renderer.text('This car', thisCarXCalc + 8, thisCarYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

      chart.renderer.label('Odometer', xAxis - 40, 164)
        .css({
          color: 'rgba(6, 41, 49, 0.4)',
        })
        .attr({
          style: 'font-size: 13px;',
          zIndex: 5
        })
        .add()
    }
  }
}
</script>