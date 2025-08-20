<template>
  <div class="progress-container">
    <div class="progress-labels">
      <span class="label">{{ label }}</span>
      <span class="percentage">{{ percentage }}%</span>
    </div>
    <div class="progress-bar">
      <div 
        class="progress-fill"
        :style="{ width: `${animatedPercentage}%` }"
        :class="`gradient-${color}`"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'

interface Props {
  label: string
  percentage: number
  color?: 'blue' | 'green' | 'purple' | 'orange' | 'pink'
  animated?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  color: 'blue',
  animated: true
})

const animatedPercentage = ref(0)

const animateProgress = () => {
  if (!props.animated) {
    animatedPercentage.value = props.percentage
    return
  }
  
  const step = props.percentage / 50
  let current = 0
  
  const interval = setInterval(() => {
    current += step
    if (current >= props.percentage) {
      animatedPercentage.value = props.percentage
      clearInterval(interval)
    } else {
      animatedPercentage.value = Math.floor(current)
    }
  }, 20)
}

onMounted(() => {
  setTimeout(animateProgress, 300)
})

watch(() => props.percentage, () => {
  animateProgress()
})
</script>

<style scoped>
.progress-container {
  @apply w-full;
}

.progress-labels {
  @apply flex justify-between mb-2;
}

.label {
  @apply text-lg font-semibold text-gray-300;
}

.percentage {
  @apply text-lg font-bold text-gray-400;
}

.progress-bar {
  @apply w-full h-3 bg-gray-700 rounded-full overflow-hidden;
}

.progress-fill {
  @apply h-full rounded-full transition-all duration-1000 ease-out;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
}

.gradient-blue {
  @apply bg-gradient-to-r from-blue-400 to-blue-600;
}

.gradient-green {
  @apply bg-gradient-to-r from-green-400 to-green-600;
}

.gradient-purple {
  @apply bg-gradient-to-r from-purple-400 to-purple-600;
}

.gradient-orange {
  @apply bg-gradient-to-r from-orange-400 to-orange-600;
}

.gradient-pink {
  @apply bg-gradient-to-r from-pink-400 to-pink-600;
}
</style>