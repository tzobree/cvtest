<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Expected days on market</h4>
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


      <div class="info">
        <p class="item"><span class="label">Day to sell:</span><span class="value">{{ avg }};</span></p>
        <p class="item"><span class="label">Lowest estimate:</span><span class="value">{{ lowest }};</span></p>
        <p class="item"><span class="label">Highest estimate:</span><span class="value">{{ highest }};</span></p>
      </div>
      
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
    lowest: {
      type: Number,
      default () {
        return 0
      }
    },
    highest: {
      type: Number,
      default () {
        return 0
      }
    },
    avg: {
      type: Number,
      default () {
        return 0
      }
    }
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
          },
          visible: false
        },
        series: [
          {
            type: 'column',
            data: [],
            // data: [
            //   [10, 12],
            //   [20, 10],
            //   [30, 13],
            //   [40, 23],
            //   [50, 35],
            //   [60, 12],
            //   [70, 33],
            //   [80, 28],
            //   [90, 21],
            // ],
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
          min: 0,
          // tickInterval: 10,
          // tickAmount: 4,
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
          title: { 
            text: 'Frequency',
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            }
          },
          labels: {
            enabled: false,
            step: 0.25,
            allowOverlap: true,
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            },
            format: '{value}%'
          },
          tickInterval: 20
        }],
        legend: {
          enabled: false
        }
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


      let avgXValue = this.avg
      let avgXCalc = xAxis.toPixels(avgXValue)
      // let avgYValue = 90
      // let avgYCalc = 310 - avgYValue * 3
      let avgYCalc = 36


      // if (avgLine) {
      //   avgLine.destroy()
      // }
      chart.renderer.path(['M', avgXCalc, yStart, 'L', avgXCalc, avgYCalc])
        .attr({
          'stroke-width': 1,
          stroke: 'orange',
          zIndex: 5
        })
        .add()

      chart.renderer.text('Avg', avgXCalc + 8, avgYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

      chart.renderer.text(avgXValue, avgXCalc + 7, avgYCalc + 48)
        .attr({
          style: 'font-size: 26px; font-weight: 500',
          zIndex: 5
        })
        .add()


      let lowestXValue = this.lowest
      let lowestXCalc = xAxis.toPixels(lowestXValue)
      // let lowestYValue = 80
      // let lowestYCalc = 310 - lowestYValue * 3
      let lowestYCalc = 96


      chart.renderer.path(['M', lowestXCalc, yStart, 'L', lowestXCalc, lowestYCalc])
        .attr({
          'stroke-width': 1,
          stroke: 'black',
          zIndex: 5
        })
        .add()
      
      chart.renderer.text('Lowest', lowestXCalc - 56, lowestYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

      chart.renderer.text(lowestXValue, lowestXCalc - 42, lowestYCalc + 48)
        .attr({
          style: 'font-size: 26px; font-weight: 500',
          zIndex: 5
        })
        .add()


      let highestXValue = this.highest
      let highestXCalc = xAxis.toPixels(highestXValue)
      // let highestYValue = 80
      // let highestYCalc = 310 - highestYValue * 3
      let highestYCalc = 96


      chart.renderer.path(['M', highestXCalc, yStart, 'L', highestXCalc, highestYCalc])
        .attr({
          'stroke-width': 1,
          stroke: 'black',
          zIndex: 5
        })
        .add()
      // if (highestLine) {
      //   highestLine.destroy()
      // }
      
      chart.renderer.text('Highest', highestXCalc + 8, highestYCalc + 20)
        .attr({
          style: 'font-size: 14px;',
          zIndex: 5
        })
        .add()

      chart.renderer.text(highestXValue, highestXCalc + 7, highestYCalc + 48)
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
    .info {
      display: flex;
      margin-top: 20px;
      .item {
        margin-right: 10px;
        .label {
          color: #062931;
          font-weight: 500;
        }
        .value {
          margin-left: 4px;
          font-weight: 400;
        }
        
      }
    }
  }
}
.highcharts-figure,
.highcharts-data-table table {
  min-width: 310px;
  max-width: 800px;
  margin: 1em auto;
}

.highcharts-data-table table {
  font-family: Verdana, sans-serif;
  border-collapse: collapse;
  border: 1px solid #ebebeb;
  margin: 10px auto;
  text-align: center;
  width: 100%;
  max-width: 500px;
}

.highcharts-data-table caption {
  padding: 1em 0;
  font-size: 1.2em;
  color: #555;
}

.highcharts-data-table th {
  font-weight: 600;
  padding: 0.5em;
}

.highcharts-data-table td,
.highcharts-data-table th,
.highcharts-data-table caption {
  padding: 0.5em;
}

.highcharts-data-table thead tr,
.highcharts-data-table tr:nth-child(even) {
  background: #f8f8f8;
}

.highcharts-data-table tr:hover {
  background: #f1f7ff;
}
</style>