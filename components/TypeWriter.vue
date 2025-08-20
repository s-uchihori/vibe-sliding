<template>
  <div class="typewriter-container">
    <span class="typed-text">{{ displayText }}</span>
    <span class="cursor" :class="{ 'cursor-blink': !isTyping }">|</span>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'

interface Props {
  text: string
  speed?: number
  delay?: number
  loop?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  speed: 100,
  delay: 0,
  loop: false
})

const displayText = ref('')
const isTyping = ref(false)
let currentIndex = 0
let timeoutId: NodeJS.Timeout | null = null

const typeText = () => {
  if (currentIndex < props.text.length) {
    isTyping.value = true
    displayText.value += props.text[currentIndex]
    currentIndex++
    timeoutId = setTimeout(typeText, props.speed)
  } else {
    isTyping.value = false
    if (props.loop) {
      setTimeout(() => {
        displayText.value = ''
        currentIndex = 0
        typeText()
      }, 2000)
    }
  }
}

const startTyping = () => {
  displayText.value = ''
  currentIndex = 0
  if (timeoutId) clearTimeout(timeoutId)
  
  setTimeout(() => {
    typeText()
  }, props.delay)
}

onMounted(() => {
  startTyping()
})

watch(() => props.text, () => {
  startTyping()
})
</script>

<style scoped>
.typewriter-container {
  @apply inline-block font-mono;
}

.typed-text {
  @apply text-2xl md:text-4xl;
  background: linear-gradient(90deg, #00c6ff 0%, #0072ff 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.cursor {
  @apply inline-block text-2xl md:text-4xl text-blue-400 ml-1;
}

.cursor-blink {
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 50% {
    opacity: 1;
  }
  51%, 100% {
    opacity: 0;
  }
}
</style>