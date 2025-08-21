<template>
  <div class="physics-container">
    <canvas ref="canvas" :width="width" :height="height"></canvas>
    <div class="controls">
      <button @click="addBall" class="btn">ボールを追加 🎾</button>
      <button @click="addBox" class="btn">ボックスを追加 📦</button>
      <button @click="reset" class="btn btn-reset">リセット 🔄</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import Matter from 'matter-js'

const canvas = ref<HTMLCanvasElement>()
const width = 800
const height = 400

let engine: Matter.Engine
let render: Matter.Render
let runner: Matter.Runner

const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8', '#FFD93D']

const getRandomColor = () => colors[Math.floor(Math.random() * colors.length)]

const addBall = () => {
  const ball = Matter.Bodies.circle(
    Math.random() * width,
    50,
    20 + Math.random() * 30,
    {
      restitution: 0.8,
      render: {
        fillStyle: getRandomColor()
      }
    }
  )
  Matter.Composite.add(engine.world, ball)
}

const addBox = () => {
  const box = Matter.Bodies.rectangle(
    Math.random() * width,
    50,
    40 + Math.random() * 40,
    40 + Math.random() * 40,
    {
      restitution: 0.6,
      angle: Math.random() * Math.PI,
      render: {
        fillStyle: getRandomColor()
      }
    }
  )
  Matter.Composite.add(engine.world, box)
}

const reset = () => {
  Matter.Composite.clear(engine.world, false)
  createBoundaries()
  
  // 初期オブジェクトを追加
  for (let i = 0; i < 3; i++) {
    addBall()
    addBox()
  }
}

const createBoundaries = () => {
  const boundaries = [
    Matter.Bodies.rectangle(width / 2, height + 25, width, 50, { isStatic: true }),
    Matter.Bodies.rectangle(width / 2, -25, width, 50, { isStatic: true }),
    Matter.Bodies.rectangle(-25, height / 2, 50, height, { isStatic: true }),
    Matter.Bodies.rectangle(width + 25, height / 2, 50, height, { isStatic: true })
  ]
  Matter.Composite.add(engine.world, boundaries)
}

onMounted(() => {
  if (!canvas.value) return

  // エンジン作成
  engine = Matter.Engine.create()
  engine.gravity.y = 1

  // レンダラー作成
  render = Matter.Render.create({
    canvas: canvas.value,
    engine: engine,
    options: {
      width,
      height,
      wireframes: false,
      background: 'transparent'
    }
  })

  // 境界を作成
  createBoundaries()

  // 初期オブジェクトを追加
  for (let i = 0; i < 3; i++) {
    addBall()
    addBox()
  }

  // マウス操作を追加
  const mouse = Matter.Mouse.create(canvas.value)
  const mouseConstraint = Matter.MouseConstraint.create(engine, {
    mouse: mouse,
    constraint: {
      stiffness: 0.2,
      render: {
        visible: false
      }
    }
  })

  Matter.Composite.add(engine.world, mouseConstraint)
  render.mouse = mouse

  // レンダリング開始
  Matter.Render.run(render)
  
  // エンジン実行
  runner = Matter.Runner.create()
  Matter.Runner.run(runner, engine)
})

onUnmounted(() => {
  if (render) {
    Matter.Render.stop(render)
  }
  if (runner) {
    Matter.Runner.stop(runner)
  }
})
</script>

<style scoped>
.physics-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
}

canvas {
  border: 2px solid #333;
  border-radius: 8px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.controls {
  display: flex;
  gap: 10px;
}

.btn {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  background: #4ECDC4;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.btn-reset {
  background: #FF6B6B;
}
</style>