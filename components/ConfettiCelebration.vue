<template>
  <div class="confetti-container">
    <h2 class="title">🎉 Celebration Time! 🎉</h2>
    <div class="buttons">
      <button @click="fireBasic" class="btn btn-basic">
        基本の紙吹雪 🎊
      </button>
      <button @click="fireRandom" class="btn btn-random">
        ランダム紙吹雪 🎨
      </button>
      <button @click="fireFromSides" class="btn btn-sides">
        両サイドから 🎭
      </button>
      <button @click="fireRealistic" class="btn btn-realistic">
        リアルな紙吹雪 ✨
      </button>
      <button @click="fireFireworks" class="btn btn-fireworks">
        花火モード 🎆
      </button>
      <button @click="fireSnow" class="btn btn-snow">
        雪モード ❄️
      </button>
      <button @click="fireStars" class="btn btn-stars">
        スターシャワー ⭐
      </button>
      <button @click="fireSchoolPride" class="btn btn-school">
        スクールプライド 🏫
      </button>
    </div>
    <div class="score-container">
      <p>クリック回数: {{ clickCount }}</p>
      <button @click="resetCount" class="btn btn-reset">リセット</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import confetti from 'canvas-confetti'

const clickCount = ref(0)

const incrementCount = () => {
  clickCount.value++
}

const resetCount = () => {
  clickCount.value = 0
}

const fireBasic = () => {
  incrementCount()
  confetti({
    particleCount: 100,
    spread: 70,
    origin: { y: 0.6 }
  })
}

const fireRandom = () => {
  incrementCount()
  const randomInRange = (min: number, max: number) => {
    return Math.random() * (max - min) + min
  }

  confetti({
    angle: randomInRange(55, 125),
    spread: randomInRange(50, 70),
    particleCount: randomInRange(50, 100),
    origin: { y: 0.6 }
  })
}

const fireFromSides = () => {
  incrementCount()
  const end = Date.now() + 1000

  const frame = () => {
    confetti({
      particleCount: 2,
      angle: 60,
      spread: 55,
      origin: { x: 0 },
      colors: ['#bb0000', '#ffffff']
    })
    confetti({
      particleCount: 2,
      angle: 120,
      spread: 55,
      origin: { x: 1 },
      colors: ['#bb0000', '#ffffff']
    })

    if (Date.now() < end) {
      requestAnimationFrame(frame)
    }
  }
  frame()
}

const fireRealistic = () => {
  incrementCount()
  const count = 200
  const defaults = {
    origin: { y: 0.7 }
  }

  const fire = (particleRatio: number, opts: any) => {
    confetti({
      ...defaults,
      ...opts,
      particleCount: Math.floor(count * particleRatio)
    })
  }

  fire(0.25, {
    spread: 26,
    startVelocity: 55,
  })
  fire(0.2, {
    spread: 60,
  })
  fire(0.35, {
    spread: 100,
    decay: 0.91,
    scalar: 0.8
  })
  fire(0.1, {
    spread: 120,
    startVelocity: 25,
    decay: 0.92,
    scalar: 1.2
  })
  fire(0.1, {
    spread: 120,
    startVelocity: 45,
  })
}

const fireFireworks = () => {
  incrementCount()
  const duration = 3000
  const animationEnd = Date.now() + duration
  const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 }

  const randomInRange = (min: number, max: number) => {
    return Math.random() * (max - min) + min
  }

  const interval: any = setInterval(() => {
    const timeLeft = animationEnd - Date.now()

    if (timeLeft <= 0) {
      return clearInterval(interval)
    }

    const particleCount = 50 * (timeLeft / duration)
    confetti({
      ...defaults,
      particleCount,
      origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 }
    })
    confetti({
      ...defaults,
      particleCount,
      origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 }
    })
  }, 250)
}

const fireSnow = () => {
  incrementCount()
  const duration = 3000
  const animationEnd = Date.now() + duration
  let skew = 1

  const randomInRange = (min: number, max: number) => {
    return Math.random() * (max - min) + min
  }

  const frame = () => {
    const timeLeft = animationEnd - Date.now()
    const ticks = Math.max(200, 500 * (timeLeft / duration))
    skew = Math.max(0.8, skew - 0.001)

    confetti({
      particleCount: 1,
      startVelocity: 0,
      ticks: ticks,
      origin: {
        x: Math.random(),
        y: (Math.random() * skew) - 0.2
      },
      colors: ['#ffffff'],
      shapes: ['circle'],
      gravity: randomInRange(0.4, 0.6),
      scalar: randomInRange(0.4, 1),
      drift: randomInRange(-0.4, 0.4)
    })

    if (timeLeft > 0) {
      requestAnimationFrame(frame)
    }
  }
  frame()
}

const fireStars = () => {
  incrementCount()
  const defaults = {
    spread: 360,
    ticks: 50,
    gravity: 0,
    decay: 0.94,
    startVelocity: 30,
    shapes: ['star'],
    colors: ['FFE400', 'FFBD00', 'E89400', 'FFCA6C', 'FDFFB8']
  }

  const shoot = () => {
    confetti({
      ...defaults,
      particleCount: 40,
      scalar: 1.2,
      shapes: ['star']
    })

    confetti({
      ...defaults,
      particleCount: 10,
      scalar: 0.75,
      shapes: ['circle']
    })
  }

  setTimeout(shoot, 0)
  setTimeout(shoot, 100)
  setTimeout(shoot, 200)
}

const fireSchoolPride = () => {
  incrementCount()
  const end = Date.now() + 3000
  const colors = ['#bb0000', '#ffffff']

  const frame = () => {
    confetti({
      particleCount: 2,
      angle: 60,
      spread: 55,
      origin: { x: 0 },
      colors: colors
    })
    confetti({
      particleCount: 2,
      angle: 120,
      spread: 55,
      origin: { x: 1 },
      colors: colors
    })

    if (Date.now() < end) {
      requestAnimationFrame(frame)
    }
  }
  frame()
}

onMounted(() => {
  // 初回マウント時に軽い紙吹雪を表示
  setTimeout(() => {
    confetti({
      particleCount: 50,
      spread: 60,
      origin: { y: 0.8 }
    })
  }, 500)
})
</script>

<style scoped>
.confetti-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
  max-height: 80vh;
  overflow-y: auto;
}

.title {
  font-size: 2rem;
  background: linear-gradient(45deg, #f093fb 0%, #f5576c 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  max-width: 700px;
  width: 100%;
}

@media (max-width: 768px) {
  .buttons {
    grid-template-columns: repeat(2, 1fr);
  }
}

.btn {
  padding: 12px 16px;
  font-size: 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 600;
  color: white;
  position: relative;
  overflow: hidden;
}

.btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.btn:hover::before {
  width: 300px;
  height: 300px;
}

.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.btn-basic {
  background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
}

.btn-random {
  background: linear-gradient(45deg, #f093fb 0%, #f5576c 100%);
}

.btn-sides {
  background: linear-gradient(45deg, #4facfe 0%, #00f2fe 100%);
}

.btn-realistic {
  background: linear-gradient(45deg, #43e97b 0%, #38f9d7 100%);
}

.btn-fireworks {
  background: linear-gradient(45deg, #fa709a 0%, #fee140 100%);
}

.btn-snow {
  background: linear-gradient(45deg, #89f7fe 0%, #66a6ff 100%);
}

.btn-stars {
  background: linear-gradient(45deg, #ffd89b 0%, #19547b 100%);
}

.btn-school {
  background: linear-gradient(45deg, #ff0844 0%, #ffb199 100%);
}

.score-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  margin-top: 10px;
}

.score-container p {
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
}

.btn-reset {
  padding: 8px 16px;
  background: linear-gradient(45deg, #ee5253 0%, #ff6b6b 100%);
}
</style>