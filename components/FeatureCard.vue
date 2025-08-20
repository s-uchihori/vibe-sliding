<template>
  <div 
    class="feature-card"
    :class="[`gradient-${color}`, { 'is-hovered': isHovered }]"
    @mouseenter="isHovered = true"
    @mouseleave="isHovered = false"
  >
    <div class="icon-wrapper">
      <slot name="icon"></slot>
    </div>
    <h3 class="title">{{ title }}</h3>
    <p class="description">{{ description }}</p>
    <div v-if="features" class="features-list">
      <div v-for="(feature, index) in features" :key="index" class="feature-item">
        <span class="feature-icon">{{ feature.icon }}</span>
        <span class="feature-text">{{ feature.text }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Feature {
  icon: string
  text: string
}

interface Props {
  title: string
  description: string
  color?: 'blue' | 'green' | 'purple' | 'orange' | 'pink'
  features?: Feature[]
}

withDefaults(defineProps<Props>(), {
  color: 'blue'
})

const isHovered = ref(false)
</script>

<style scoped>
.feature-card {
  @apply relative p-8 rounded-2xl transition-all duration-300 transform;
  @apply hover:scale-105 hover:shadow-2xl cursor-pointer;
}

.gradient-blue {
  @apply bg-gradient-to-br from-blue-500 to-indigo-600;
}

.gradient-green {
  @apply bg-gradient-to-br from-green-500 to-teal-600;
}

.gradient-purple {
  @apply bg-gradient-to-br from-purple-500 to-pink-600;
}

.gradient-orange {
  @apply bg-gradient-to-br from-orange-500 to-red-600;
}

.gradient-pink {
  @apply bg-gradient-to-br from-pink-500 to-rose-600;
}

.icon-wrapper {
  @apply text-6xl mb-4 text-white/90;
}

.title {
  @apply text-2xl font-bold text-white mb-3;
}

.description {
  @apply text-white/80 text-base;
}

.features-list {
  @apply mt-4 space-y-2;
}

.feature-item {
  @apply flex items-center gap-2 text-white/90;
}

.feature-icon {
  @apply text-lg;
}

.feature-text {
  @apply text-sm;
}

.is-hovered {
  @apply rotate-1;
}
</style>