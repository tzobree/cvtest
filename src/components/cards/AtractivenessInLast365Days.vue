<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Atractiveness in last 365 days</h4>
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
              }
            },
            reversed: true
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
          }
        ],
        plotOptions: {
          spline: {
            color: '#FFC063'
          }
        },
        series: [
          {
            name: '',
            type: 'spline',
            color: '#FFC063',
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


      let thisCarXValue = 0
      let thisCarXCalc = xAxis.toPixels(thisCarXValue)
      // let thisCarYValue = 30
      // let thisCarYCalc = 160 - thisCarYValue * 3
      let thisCarYCalc = 10


      chart.renderer.path(['M', thisCarXCalc, yStart - 49, 'L', thisCarXCalc, thisCarYCalc])
        .attr({
          'stroke-width': 2,
          stroke: '#000',
          zIndex: 5
        })
        .add()
      
      chart.renderer.text('Today', thisCarXCalc - 60, thisCarYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

    }
  }
}
</script>