<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Price per next km driven</h4>
        <span class="tooltip">icon</span>
      </div>
      <div class="right">
        <span>arrow</span>
      </div>
    </div>
    <div class="card-content">

      <HighChart :options="chartNextKm"
      class="highcharts" ref="chartRef"></HighChart>

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
      chartNextKm: {
        title: null,
        credits: {
          enabled: false
        },
        chart: {
          height: 350
        },
        series: [
          {
            type: 'spline',
            // data: [10600, 10574, 10123, 8569],
            // data: [
            //   [10, 10600],
            //   [5000, 10574],
            //   [20000, 10123],
            //   [50000, 8569]
            // ],
            data: [],
            zIndex: 1,
            color: '#FFC063',
            marker: {
              radius: 5,
              fillColor: '#062931'
            },
            dataLabels: {
              enabled: true,
              color: 'rgba(6, 41, 49, 0.92)',
              style: {
                fontSize: 14,
                fontFamily: 'DM Sans',
                fontWeight: 400
              },
              y: -5,
              formatter: function () {
                // if (this.point.category === 10) {
                //   return ''
                // } else {
                //   return this.point.options.y
                // }
                return this.point.options.y.toLocaleString('en-US')
              }
            }
          },
        ],
        xAxis: [{
          maxPadding: 0.05,
          min: 1,
          gridLineDashStyle: 'solid',
          gridLineWidth: 0.5,
          gridLineColor: 'rgba(6, 41, 49, 0.1)',
          // tickInterval: 5000,
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
          showFirstLabel: true,
          lineColor: '#062931',
          // type: 'logarithmic',
          // categories: ["1", "1000", "5000", "50000"],
        }],
        yAxis: [{
          min: 0,
          // max: 19,
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
            }
          },
          title: {
            enabled: false
          },
          tickInterval: 2000
        }],
        legend: {
          enabled: false
        }
      },
    }
  },
  mounted () {
    let chart = this.$refs.chartRef.chart
    // console.log('chart', chart)
    let calcData = this.chartData.map(item => {
      item[1] = parseInt(item[1])
      return item
    })
    chart.update({
      series: {
        data: calcData
      }
    })
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