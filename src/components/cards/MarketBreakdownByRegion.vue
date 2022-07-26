<template>
  <div class="report-card">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Market breakdown by region</h4>
        <span class="tooltip">icon</span>
      </div>
      <div class="right">
        <span>arrow</span>
      </div>
    </div>
    <div class="card-content">

      <div class="grid regions-grid">
        <div class="column">
          <ul class="list">
            <li class="region-row" v-for="region in tableData" :key="region.name">
              <div class="point" :style="{ backgroundColor: region.color }"></div>
              <p class="name">{{ region.name }}</p>
              <p class="cars">{{ region.cars }} cars</p>
              <p class="percent">{{ region.percent }}%</p>
            </li>
          </ul>
        </div>
        <div class="column">
          <HighChart
          :options="chartOptions" ref="chartRef"
          class="highcharts">
          </HighChart>
        </div>
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
  data () {
    return {
      chartOptions: {
        chart: {
          plotBackgroundColor: null,
          plotBorderWidth: null,
          plotShadow: false,
          type: 'pie',
          height: 280
        },
        title: {
          text: '',
          enabled: false
        },
        tooltip: {
          pointFormat: '{series.name}: <b>{point.percentage:.1f}%</b>'
        },
        accessibility: {
          point: {
            valueSuffix: '%'
          }
        },
        plotOptions: {
          pie: {
            allowPointSelect: true,
            cursor: 'pointer',
            dataLabels: {
              enabled: true,
              format: '{point.percentage:.1f} %',
              connectorWidth: 1,
              distance: 5
            }
          }
        },
        series: [{
          name: 'Brands',
          colorByPoint: true,
          data: [
            30.6, 22.2, 37.0, 12.9, 8, 3
          ]
        }],
        colors: [
          '#FF6359', '#FFC1BD', '#DEE0E5', '#104756', '#51E6FB', '#F1F5F8'
        ]
      },
      tableData: [],
      finalChartData: []
    }
  },
  created () {
    const regions = [
      'Auckland', 'Bay of Plenty', 'Canterbury', 'Wellington', 'Gisborne', 'Other regions'
    ]
    const someData = [
      30, 22, 37, 12, 8, 3
    ]
    let sum = 0
    for (let i = 0; i < someData.length; i++) {
        sum += someData[i]
    }
    for (let i = 0; i < someData.length; i++) {
      this.tableData[i] = {}
      this.tableData[i].color = this.chartOptions.colors[i]
      this.tableData[i].name = regions[i]
      this.tableData[i].cars = someData[i]
      this.tableData[i].percent = ((someData[i]/sum)*100).toFixed(1)
    }
    console.log('regions', this.tableData)

    // let finalChartData = []
    for (let i = 0; i < this.tableData.length; i++) {
      this.finalChartData[i] = this.tableData[i].percent
    }
  },
  mounted () {
    let chart = this.$refs.chartRef.chart
    chart.update({
      series: {
        data: this.finalChartData
      }
    })
  }
}
</script>

<style lang="scss">
.report-body {
  .report-card {
    .card-content {
      padding-top: 72px;
      .grid.regions-grid {
        margin-top: 0;
        .column {
          .list {
            list-style: none;
            padding-left: 0;
            .region-row {
              display: grid;
              grid-template-columns: 1fr 6fr 5fr 4fr;
              align-items: center;
              padding: 11px 0;
              border-bottom: 1px solid rgba(6, 41, 49, 0.15);
              font-size: 1rem;
              p {
                margin: 0;
              }
              &:last-child {
                border: none;
              }
              .point {
                height: 10px;
                width: 10px;
                box-sizing: border-box;
                border-radius: 50%;
                border: 1px solid #DEE0E5;

              }
              .name {
                color: #062931;
                font-weight: 700;
              }
              .cars, .percent {
                color: rgba(6, 41, 49, 0.92);
                text-align: right;
              }
            }
          }
        }
      }
    }
  }
}
</style>