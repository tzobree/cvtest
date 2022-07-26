<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">3 Years projected price</h4>
        <span class="tooltip">icon</span>
      </div>
      <div class="right">
        <span>arrow</span>
      </div>
    </div>
    <div class="card-content">

      <HighChart
      :options="chartOptions"
      class="highcharts mb" ref="chartRef"></HighChart>

      <div class="grid">
        <div class="column">
          <ul class="list">
            <li v-for="row in tableColumn1" :key="row.label" class="row">
              <p class="label">{{ row.label }}</p>
              <p class="value">$ {{ row.price }}</p>
            </li>
          </ul>
        </div>
        <div class="column">
          <ul class="list">
            <li v-for="row in tableColumn2" :key="row.label"  class="row">
              <p class="label">{{ row.label }}</p>
              <p class="value">$ {{ row.price }}</p>
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
  props: {
    chartData: {
      type: Array,
      default () {
        return  []
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
        tooltip: {
          enabled: false
        },
        chart: {
          height: 350
        },
        plotOptions: {
          series: {
            // enableMouseTracking: false
          }
        },
        series: [
          {
            type: 'spline',
            // data: [17, 15, 13.2, 11.5, 10.1, 9],
            data: [],
            zIndex: 1,
            color: '#FFC063',
            marker: {
              radius: 6,
              fillColor: '#062931'
            },
            stickyTracking: false,
            dataLabels: {
              enabled: false,
              color: 'rgba(6, 41, 49, 0.92)',
              style: {
                fontSize: 15,
                fontFamily: 'DM Sans',
                fontWeight: 400
              },
              y: -5,
              formatter: function () {
                // console.log(this)
                return '$' + this.y.toLocaleString('en-US')
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
            },
            states: { hover: { enabled: true } }, // Here is where it goes
          },
          {
            name: 'Range',
            type: 'areasplinerange',
            // data: [
            //   [17.2, 16.9],
            //   [15.3, 14.7],
            //   [13.6, 12.7],
            //   [12.1, 10.9],
            //   [10.9, 9.4],
            //   [10, 8],
            // ],
            data: [],
            enableMouseTracking: false,
            lineWidth: 0,
            color: '#FFF5DC',
            fillOpacity: 1,
            zIndex: 0,
            marker: {
              enabled: false
            },
            stickyTracking: false,
            dataLabels: {
              enabled: true,
              color: 'rgba(6, 41, 49, 0.92)',
              style: {
                fontSize: 15,
                fontFamily: 'DM Sans',
                fontWeight: 400
              },
              crop: false,
              overflow: 'allow',
              formatter: function () {
                if (this.point.below) {
                  return ''
                } else {
                  return (this.point.options.low/1000).toFixed(1) + ' - ' + (this.point.options.high/1000).toFixed(1) + ' k'
                }
              }
            }
          }
        ],
        xAxis: [{
          categories: ["Today", "30 days", "180 days", "1 year", "2 years", "3 years"],
          minPadding: 0.1,
          maxPadding: 0.1,
          gridLineDashStyle: 'solid',
          gridLineWidth: 0.5,
          gridLineColor: 'rgba(6, 41, 49, 0.1)',
          tickInterval: 0.5,
          tickAmount: 6,
          tickLength: 0,
          labels: {
            step: 2,
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            }
          },
          lineColor: '#062931'
        }],
        yAxis: [{
          // min: 6,
          // max: 19,
          gridLineDashStyle: 'solid',
          gridLineWidth: 0.5,
          gridLineColor: 'rgba(6, 41, 49, 0.1)',
          labels: {
            // format: '$ {value}',
            formatter: function () {
              return '$ ' + this.value.toLocaleString('en-US')
            },
            allowOverlap: true,
            style: {
              color: 'rgba(6, 41, 49, 0.4)',
              fontSize: 13,
              fontFamily: 'DM Sans'
            }
          },
          title: {
            enabled: false
          },
          // tickInterval: 2
        }],
        legend: {
          enabled: false
        }
      },
      tableData: [],
      tableColumn1: [],
      tableColumn2: [],
      splineData: [],
      rangeData: []
    }
  },
  mounted () {
    this.tableColumn1 = []
    this.tableColumn2 = []
    for (let i = 0; i < 6; i++) {

      if (i < 3) {
        // this.tableColumn1.push({
        //   label: this.chartOptions.xAxis[0].categories[i],
        //   price: Math.round(this.chartData[i][3])
        // })
        this.tableColumn1[i] = {
          label: this.chartOptions.xAxis[0].categories[i],
          price: Math.round(this.chartData[i][3]).toLocaleString('en-US')
        }
      } else {
        // this.tableColumn2.push({
        //   label: this.chartOptions.xAxis[0].categories[i],
        //   price: Math.round(this.chartData[i][3])
        // })
        this.tableColumn2[i - 3] = {
          label: this.chartOptions.xAxis[0].categories[i],
          price: Math.round(this.chartData[i][3]).toLocaleString('en-US')
        }
      }

      // this.splineData.push(Math.round(this.chartData[i][3]))
      this.splineData[i] = Math.round(this.chartData[i][3])
      this.rangeData[i] = [
        Math.round(this.chartData[i][1]), 
        Math.round(this.chartData[i][5])
      ]
    }
    let chart = this.$refs.chartRef.chart
    chart.update({
      series: [
        {
          type: 'spline',
          data: this.splineData
        },
        {
          type: 'areasplinerange',
          data: this.rangeData
        }
      ]
    })
    // chart.series.forEach(series => series.update({
    //   data: series.type === 'spline' ? this.splineData : []
    // }, false))
    // chart.redraw()
  },
  watch: {
    chartData: {
      immediate: true,
      handler () {
        // console.log('table data', this.tableData)
        // console.log('spline data', this.splineData)
      }
    }
  },
  methods: {

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