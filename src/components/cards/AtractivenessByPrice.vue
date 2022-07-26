<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Atractiveness by price</h4>
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
            },
            min: 0
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
            min: 0
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
          {
            name: '',
            type: 'spline',
            color: 'black',
            data: [],
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

      chart.renderer.label('Price', xAxis - 40, 164)
        .css({
          color: 'rgba(6, 41, 49, 0.4)',
        })
        .attr({
          style: 'font-size: 13px;',
          zIndex: 5
        })
        .add()

    },
  }
}
</script>

<style>
.custom-xaxis-title {
  color: green;
}
</style>