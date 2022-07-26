<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Market breakdown by travelled km</h4>
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

      <div class="grid">
        <div class="column">
          <ul class="list">
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
          </ul>
        </div>
        <div class="column">
          <ul class="list">
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
          </ul>
        </div>
      </div>
      
    </div>
  </div>
</template>
<script>
import { Chart } from 'highcharts-vue'
import Highcharts from 'highcharts'
import more from 'highcharts/highcharts-more'
// import histogram from 'highcharts/modules/histogram-bellcurve'

more(Highcharts)
// histogram(Highcharts)

export default {
  components: {
    HighChart: Chart,
  },
  data () {
    return {
      chartOptions: {
        title: null,
        credits: {
          enabled: false
        },
        chart: {
          height: 350
        },
        tooltip: {
          enabled: false
        },
        plotOptions: {
          column: {
            cursor: 'pointer',
            states: {
              hover: {
                color: '#FFF1CD'
              }
            }
          }
        },
        series: [
          {
            type: 'column',
            data: [
              [10000, 22],
              [20000, 40],
              [30000, 33],
              [40000, 23],
              [50000, 35],
              [60000, 12],
              [70000, 0],
              [80000, 28],
              [90000, 21],
              [100000, 11],
              [110000, 1]
            ],
            pointPlacement: -0.5,
            pointWidth: 60,
            color: '#FFF5DC',
            style: {
              color: '#FFF5DC'
            },
            zIndex: 0,
            dataLabels: {
              enabled: false,
              formatter: function () {
                // console.log(this)
                return this.y + '%'
              },
              style: {
                fontSize: 15,
                fontFamily: 'DM Sans',
                fontWeight: 300
              }
            },
            point: {
              events: {
                mouseOver: function () {
                  this.update({
                    dataLabels: {
                      enabled: true
                    }
                  })
                },
                mouseOut: function () {
                  this.update({
                    dataLabels: {
                      enabled: false
                    }
                  })
                }
              }
            }
          },
        ],
        xAxis: [{
          maxPadding: -0.03,
          minPadding: 0,
          min: 3000,
          tickInterval: 10000,
          // tickAmount: 10,
          tickLength: 0,
          labels: {
            step: 1,
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            }
          },
          // showFirstLabel: true,
          lineColor: '#062931',
        }],
        yAxis: [{
          min: 0,
          max: 100,
          gridLineDashStyle: 'solid',
          gridLineWidth: 0.5,
          gridLineColor: 'rgba(6, 41, 49, 0.1)',
          labels: {
            // format: '$ {value}000',
            step: 0.25,
            allowOverlap: true,
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            },
            format: '{value}%'
          },
          title: {
            enabled: false
          },
          tickInterval: 20
        }],
        legend: {
          enabled: false
        }
      },
    }
  },
  mounted () {
    this.drawElements()
    // this.atractivenessByPriceDrawLines()
  },
  methods: {
    drawElements () {
      const chart = this.$refs.chartRef.chart
      let xAxis = chart.xAxis[0]
      let yAxis = chart.yAxis[0]
      let yStart = yAxis.toPixels(0)

      let avgXValue = 49400
      let avgXCalc = xAxis.toPixels(avgXValue)
      let avgYValue = 65
      let avgYCalc = 310 - avgYValue * 3

      if (avgLine) {
        avgLine.destroy()
      }
      if (avgLabel) {
        avgLabel.destroy()
      }
      if (avgValue) {
        avgValue.destroy()
      }

      const avgLine = chart.renderer.path(['M', avgXCalc, yStart, 'L', avgXCalc, avgYCalc])
        .attr({
          'stroke-width': 1,
          stroke: '#062931',
          zIndex: 5
        })
        .add()
      
      const avgLabel = chart.renderer.text('Avg', avgXCalc + 8, avgYCalc + 20)
        .attr({
          style: 'font-size: 15px;',
          zIndex: 5
        })
        .add()

      const avgValue = chart.renderer.text('49.4 k', avgXCalc + 7, avgYCalc + 48)
        .attr({
          style: 'font-size: 26px; font-weight: 500',
          zIndex: 5
        })
        .add()


      let thisXValue = 90000
      let thisXCalc = xAxis.toPixels(thisXValue)
      let thisYValue = 90
      let thisYCalc = 310 - thisYValue * 3

      if (thisLine) {
        thisLine.destroy()
      }
      if (thisLabel) {
        thisLabel.destroy()
      }
      if (thisValue) {
        thisValue.destroy()
      }

      const thisLine = chart.renderer.path(['M', thisXCalc, yStart, 'L', thisXCalc, thisYCalc])
        .attr({
          'stroke-width': 1,
          stroke: 'orange',
          zIndex: 5
        })
        .add()
      
      const thisLabel = chart.renderer.text('This car', thisXCalc + 8, thisYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

      const thisValue = chart.renderer.text('90 k', thisXCalc + 7, thisYCalc + 48)
        .attr({
          style: 'font-size: 26px; font-weight: 500',
          zIndex: 5
        })
        .add()

    },
  }
}
</script>

<style lang="scss">
.report-card {
  .card-content {
    padding-top: 72px;
  }
}

</style>