<template>
  <div class="relative w-screen h-screen overflow-hidden bg-white">
    <!-- SVG Filter -->
    <svg class="absolute w-0 h-0" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <filter id="goo">
          <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
          <feColorMatrix 
            in="blur" 
            mode="matrix" 
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -8" 
            result="goo" 
          />
          <feBlend in="SourceGraphic" in2="goo" />
        </filter>
      </defs>
    </svg>

    <!-- Gradient Background -->
    <div class="absolute inset-0 bg-white">
      <!-- Main Gradients Container -->
      <div 
        class="absolute inset-0 w-full h-full blur-[50px]"
        style="filter: url(#goo) blur(50px)"
      >
        <!-- Gradient 1 - Vertical Movement -->
        <div 
          ref="g1"
          class="absolute w-[85%] h-[85%] rounded-full opacity-30 mix-blend-multiply animate-move-vertical"
          :style="gradient1Style"
        />
        
        <!-- Gradient 2 - Circle Movement -->
        <div 
          ref="g2"
          class="absolute w-[85%] h-[85%] rounded-full opacity-30 mix-blend-multiply animate-move-circle-reverse"
          :style="gradient2Style"
        />
        
        <!-- Gradient 3 - Circle Movement Slow -->
        <div 
          ref="g3"
          class="absolute w-[85%] h-[85%] rounded-full opacity-30 mix-blend-multiply animate-move-circle-slow"
          :style="gradient3Style"
        />
        
        <!-- Gradient 4 - Horizontal Movement -->
        <div 
          ref="g4"
          class="absolute w-[85%] h-[85%] rounded-full opacity-25 mix-blend-multiply animate-move-horizontal"
          :style="gradient4Style"
        />
        
        <!-- Gradient 5 - Large Circle -->
        <div 
          ref="g5"
          class="absolute w-[170%] h-[170%] rounded-full opacity-30 mix-blend-multiply animate-move-circle"
          :style="gradient5Style"
        />
        
        <!-- Interactive Gradient 1 - Follows Mouse -->
        <div 
          ref="interactive1"
          class="absolute w-[100%] h-[100%] rounded-full opacity-35 mix-blend-multiply transition-transform duration-75 ease-out"
          :style="interactive1Style"
        />
        
        <!-- Interactive Gradient 2 - Follows Mouse with Delay -->
        <div 
          ref="interactive2"
          class="absolute w-[90%] h-[90%] rounded-full opacity-30 mix-blend-multiply transition-transform duration-150 ease-out"
          :style="interactive2Style"
        />
        
        <!-- Interactive Gradient 3 - Follows Mouse with More Delay -->
        <div 
          ref="interactive3"
          class="absolute w-[80%] h-[80%] rounded-full opacity-25 mix-blend-multiply transition-transform duration-200 ease-out"
          :style="interactive3Style"
        />
      </div>
    </div>

    <!-- Text Content -->
    <div class="absolute inset-0 flex items-center justify-center z-10 pointer-events-none select-none">
      <h1 class="text-8xl md:text-9xl font-light text-gray-700 opacity-80 tracking-wide" style="font-family: 'Dongle', sans-serif">
        Hello
      </h1>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

// Mouse position
const mouseX = ref(0)
const mouseY = ref(0)

// Interactive bubble positions with different delays
const curX1 = ref(0)
const curY1 = ref(0)
const curX2 = ref(0)
const curY2 = ref(0)
const curX3 = ref(0)
const curY3 = ref(0)

// Animation frame ID
let animationFrameId = null

// Gradient styles
const gradient1Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(156, 163, 175, 0.6) 0%, rgba(156, 163, 175, 0) 50%)',
  top: 'calc(50% - 42.5%)',
  left: 'calc(50% - 42.5%)',
  transformOrigin: 'center center'
}))

const gradient2Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(209, 213, 219, 0.6) 0%, rgba(209, 213, 219, 0) 50%)',
  top: 'calc(50% - 42.5%)',
  left: 'calc(50% - 42.5%)',
  transformOrigin: 'calc(50% - 400px)'
}))

const gradient3Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(229, 231, 235, 0.6) 0%, rgba(229, 231, 235, 0) 50%)',
  top: 'calc(50% - 42.5% + 200px)',
  left: 'calc(50% - 42.5% - 500px)',
  transformOrigin: 'calc(50% + 400px)'
}))

const gradient4Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(156, 163, 175, 0.5) 0%, rgba(156, 163, 175, 0) 50%)',
  top: 'calc(50% - 42.5%)',
  left: 'calc(50% - 42.5%)',
  transformOrigin: 'calc(50% - 200px)'
}))

const gradient5Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(209, 213, 219, 0.6) 0%, rgba(209, 213, 219, 0) 50%)',
  top: 'calc(50% - 85%)',
  left: 'calc(50% - 85%)',
  transformOrigin: 'calc(50% - 800px) calc(50% + 200px)'
}))

const interactive1Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(156, 163, 175, 0.7) 0%, rgba(156, 163, 175, 0) 50%)',
  transform: `translate(${Math.round(curX1.value)}px, ${Math.round(curY1.value)}px)`,
  top: '-50%',
  left: '-50%'
}))

const interactive2Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(209, 213, 219, 0.6) 0%, rgba(209, 213, 219, 0) 50%)',
  transform: `translate(${Math.round(curX2.value)}px, ${Math.round(curY2.value)}px)`,
  top: '-45%',
  left: '-45%'
}))

const interactive3Style = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(229, 231, 235, 0.5) 0%, rgba(229, 231, 235, 0) 50%)',
  transform: `translate(${Math.round(curX3.value)}px, ${Math.round(curY3.value)}px)`,
  top: '-40%',
  left: '-40%'
}))

// Smooth animation loop
const animate = () => {
  // Interactive bubble 1 - fastest
  curX1.value += (mouseX.value - curX1.value) / 15
  curY1.value += (mouseY.value - curY1.value) / 15
  
  // Interactive bubble 2 - medium speed
  curX2.value += (mouseX.value - curX2.value) / 25
  curY2.value += (mouseY.value - curY2.value) / 25
  
  // Interactive bubble 3 - slowest
  curX3.value += (mouseX.value - curX3.value) / 35
  curY3.value += (mouseY.value - curY3.value) / 35
  
  animationFrameId = requestAnimationFrame(animate)
}

// Mouse move handler
const handleMouseMove = (event) => {
  mouseX.value = event.clientX
  mouseY.value = event.clientY
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
  animate()
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId)
  }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Dongle:wght@300;400;700&display=swap');

@keyframes move-vertical {
  0%, 100% {
    transform: translateY(-50%);
  }
  50% {
    transform: translateY(50%);
  }
}

@keyframes move-circle {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes move-circle-reverse {
  0% {
    transform: rotate(360deg);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(0deg);
  }
}

@keyframes move-circle-slow {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes move-horizontal {
  0%, 100% {
    transform: translateX(-50%) translateY(-10%);
  }
  50% {
    transform: translateX(50%) translateY(10%);
  }
}

.animate-move-vertical {
  animation: move-vertical 30s ease infinite;
}

.animate-move-circle {
  animation: move-circle 20s ease infinite;
}

.animate-move-circle-reverse {
  animation: move-circle-reverse 20s ease infinite;
}

.animate-move-circle-slow {
  animation: move-circle-slow 40s linear infinite;
}

.animate-move-horizontal {
  animation: move-horizontal 40s ease infinite;
}
</style>