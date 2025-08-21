<template>
  <span ref="elementRef">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { annotate } from 'rough-notation'
import type { RoughAnnotation, RoughAnnotationType } from 'rough-notation/lib/model'

const props = withDefaults(defineProps<{
  type?: RoughAnnotationType
  color?: string
  animate?: boolean
  animationDuration?: number
  strokeWidth?: number
  padding?: number | [number, number, number, number]
  multiline?: boolean
  iterations?: number
  show?: boolean
}>(), {
  type: 'underline',
  color: 'currentColor',
  animate: true,
  animationDuration: 800,
  strokeWidth: 1,
  padding: 5,
  multiline: false,
  iterations: 2,
  show: true
})

const elementRef = ref<HTMLElement>()
let annotation: RoughAnnotation | null = null

onMounted(() => {
  if (elementRef.value) {
    annotation = annotate(elementRef.value, {
      type: props.type,
      color: props.color,
      animate: props.animate,
      animationDuration: props.animationDuration,
      strokeWidth: props.strokeWidth,
      padding: props.padding,
      multiline: props.multiline,
      iterations: props.iterations,
    })
    
    if (props.show) {
      annotation.show()
    }
  }
})

watch(() => props.show, (newVal) => {
  if (annotation) {
    if (newVal) {
      annotation.show()
    } else {
      annotation.hide()
    }
  }
})
</script>