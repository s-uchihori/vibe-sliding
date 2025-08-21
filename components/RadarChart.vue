<template>
  <div class="chart-container">
    <Radar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { Radar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  RadialLinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
} from 'chart.js'

ChartJS.register(
  RadialLinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
)

interface Props {
  title?: string
  labels?: string[]
  datasets?: any[]
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Skills Assessment',
  labels: () => ['Frontend', 'Backend', 'DevOps', 'Database', 'Mobile', 'Security'],
  datasets: () => [
    {
      label: 'Current Level',
      data: [95, 80, 75, 70, 60, 85],
      borderColor: 'rgba(139, 92, 246, 1)',
      backgroundColor: 'rgba(139, 92, 246, 0.2)',
      borderWidth: 2,
      pointBackgroundColor: 'rgba(139, 92, 246, 1)',
      pointBorderColor: '#fff',
      pointHoverBackgroundColor: '#fff',
      pointHoverBorderColor: 'rgba(139, 92, 246, 1)'
    },
    {
      label: 'Target Level',
      data: [100, 90, 85, 80, 75, 90],
      borderColor: 'rgba(34, 197, 94, 1)',
      backgroundColor: 'rgba(34, 197, 94, 0.2)',
      borderWidth: 2,
      pointBackgroundColor: 'rgba(34, 197, 94, 1)',
      pointBorderColor: '#fff',
      pointHoverBackgroundColor: '#fff',
      pointHoverBorderColor: 'rgba(34, 197, 94, 1)'
    }
  ]
})

const chartData = {
  labels: props.labels,
  datasets: props.datasets
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: 'top' as const,
      labels: {
        color: '#fff',
        font: {
          size: 14
        }
      }
    },
    title: {
      display: true,
      text: props.title,
      color: '#fff',
      font: {
        size: 18,
        weight: 'bold' as const
      }
    }
  },
  scales: {
    r: {
      angleLines: {
        color: 'rgba(255, 255, 255, 0.2)'
      },
      grid: {
        color: 'rgba(255, 255, 255, 0.2)'
      },
      pointLabels: {
        color: '#fff',
        font: {
          size: 12
        }
      },
      ticks: {
        color: '#fff',
        backdropColor: 'transparent'
      },
      suggestedMin: 0,
      suggestedMax: 100
    }
  }
}
</script>

<style scoped>
.chart-container {
  position: relative;
  height: 400px;
  width: 100%;
}
</style>