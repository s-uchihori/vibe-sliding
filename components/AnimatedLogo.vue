<template>
  <div class="logo-container">
    <div class="logo-wrapper">
      <div class="logo-text">
        <span 
          v-for="(char, index) in logoChars" 
          :key="index"
          class="logo-char"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          {{ char }}
        </span>
      </div>
      <div class="logo-subtitle">{{ subtitle }}</div>
    </div>
    <div class="particles">
      <div 
        v-for="n in 20" 
        :key="n" 
        class="particle"
        :style="{
          left: `${Math.random() * 100}%`,
          animationDelay: `${Math.random() * 5}s`,
          animationDuration: `${5 + Math.random() * 10}s`
        }"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

interface Props {
  text?: string
  subtitle?: string
}

const props = withDefaults(defineProps<Props>(), {
  text: 'Slidev',
  subtitle: 'Presentation Framework'
})

const logoChars = computed(() => props.text.split(''))
</script>

<style scoped>
.logo-container {
  @apply relative flex items-center justify-center h-full;
}

.logo-wrapper {
  @apply relative z-10 text-center;
}

.logo-text {
  @apply text-6xl md:text-8xl font-bold;
}

.logo-char {
  @apply inline-block;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: wave 2s ease-in-out infinite;
}

.logo-subtitle {
  @apply text-2xl mt-4 text-gray-300;
  animation: fadeIn 1s ease-in-out;
}

.particles {
  @apply absolute inset-0 overflow-hidden;
}

.particle {
  @apply absolute w-2 h-2 bg-purple-400 rounded-full opacity-40;
  animation: float linear infinite;
}

@keyframes wave {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {
  from {
    transform: translateY(100vh) rotate(0deg);
    opacity: 0;
  }
  10% {
    opacity: 0.4;
  }
  90% {
    opacity: 0.4;
  }
  to {
    transform: translateY(-100vh) rotate(360deg);
    opacity: 0;
  }
}
</style>