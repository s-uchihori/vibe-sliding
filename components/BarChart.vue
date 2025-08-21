<template>
  <div class="chart-container">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'

ChartJS.register(
  CategoryScale,
  LinearScale,
  BarElement,
  Title,
  Tooltip,
  Legend
)

interface Props {
  title?: string
  labels?: string[]
  datasets?: any[]
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Technology Adoption',
  labels: () => ['Vue', 'React', 'Angular', 'Svelte', 'Solid'],
  datasets: () => [
    {
      label: '2023',
      data: [88, 95, 72, 45, 25],
      backgroundColor: [
        'rgba(139, 92, 246, 0.8)',
        'rgba(59, 130, 246, 0.8)',
        'rgba(239, 68, 68, 0.8)',
        'rgba(251, 146, 60, 0.8)',
        'rgba(34, 197, 94, 0.8)'
      ],
      borderColor: [
        'rgb(139, 92, 246)',
        'rgb(59, 130, 246)',
        'rgb(239, 68, 68)',
        'rgb(251, 146, 60)',
        'rgb(34, 197, 94)'
      ],
      borderWidth: 2
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
    x: {
      grid: {
        color: 'rgba(255, 255, 255, 0.1)'
      },
      ticks: {
        color: '#fff'
      }
    },
    y: {
      beginAtZero: true,
      grid: {
        color: 'rgba(255, 255, 255, 0.1)'
      },
      ticks: {
        color: '#fff'
      }
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