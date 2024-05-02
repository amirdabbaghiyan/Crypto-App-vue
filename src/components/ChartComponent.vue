<template>
    <div style="position: relative; height:40vh; width:80vw">
        <line-chart :chart-data="chartData" :options="chartOptions"/>
    </div>
</template>

<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: ['data', 'type'],
  computed: {
    chartData() {
      return {
        labels: this.data.map(d => d.date),
        datasets: [
          {
            label: this.type,
            backgroundColor: 'var(--primary-color)',
            borderColor: 'var(--primary-color)',
            data: this.data.map(d => d[this.type]),
            fill: false,
          }
        ]
      }
    },
    chartOptions() {
      return {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          yAxes: [{
            ticks: {
              beginAtZero: true,
              suggestedMax: Math.max(...this.data.map(d => d[this.type])) + 1
            }
          }],
          xAxes: [{
            display: false
          }]
        },
        legend: {
          display: true
        },
        tooltips: {
          enabled: true
        }
      }
    }
  },
  mounted () {
    this.renderChart(this.chartData, this.chartOptions)
  }
}
</script>

<style scoped>
.primary-color {
  --primary-color: #3498db;
}
</style>