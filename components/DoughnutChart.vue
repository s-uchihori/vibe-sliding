<template>
  <div class="chart-container">
    <Doughnut :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { Doughnut } from 'vue-chartjs'
import {
  Chart as ChartJS,
  ArcElement,
  Tooltip,
  Legend
} from 'chart.js'

ChartJS.register(ArcElement, Tooltip, Legend)

interface Props {
  title?: string
  labels?: string[]
  data?: number[]
  colors?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Market Share',
  labels: () => ['JavaScript', 'Python', 'Java', 'TypeScript', 'Go'],
  data: () => [35, 25, 20, 15, 5],
  colors: () => [
    'rgba(251, 146, 60, 0.8)',
    'rgba(59, 130, 246, 0.8)',
    'rgba(239, 68, 68, 0.8)',
    'rgba(139, 92, 246, 0.8)',
    'rgba(34, 197, 94, 0.8)'
  ]
})

const chartData = {
  labels: props.labels,
  datasets: [
    {
      data: props.data,
      backgroundColor: props.colors,
      borderColor: props.colors.map(c => c.replace('0.8', '1')),
      borderWidth: 2
    }
  ]
}

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: 'right' as const,
      labels: {
        color: '#fff',
        font: {
          size: 14
        },
        padding: 20
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