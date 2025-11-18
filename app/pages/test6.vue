<template>
  <div class="relative w-full min-h-screen overflow-hidden bg-white">
    <!-- Morphing Gradient Background -->
    <div 
      ref="gradientContainer"
      class="fixed inset-0 w-full h-full"
    >
      <!-- SVG Filter for organic morphing -->
      <svg class="absolute inset-0 w-full h-full pointer-events-none">
        <defs>
          <filter id="blob-filter">
            <feTurbulence
              :baseFrequency="turbulenceFrequency"
              numOctaves="3"
              result="turbulence"
              seed="2"
            >
              <animate
                attributeName="baseFrequency"
                :values="turbulenceAnimation"
                dur="20s"
                repeatCount="indefinite"
              />
            </feTurbulence>
            <feDisplacementMap
              in="SourceGraphic"
              in2="turbulence"
              :scale="displacementScale"
              xChannelSelector="R"
              yChannelSelector="G"
            />
            <feGaussianBlur :stdDeviation="blurAmount" />
          </filter>
        </defs>
      </svg>

      <!-- Blob layers -->
      <div
        v-for="(blob, index) in blobs"
        :key="index"
        class="absolute rounded-full will-change-transform pointer-events-none"
        :class="blob.blendMode"
        :style="{
          left: `calc(${blob.x}% + ${mouseInfluence[index]?.x || 0}px)`,
          top: `calc(${blob.y}% + ${mouseInfluence[index]?.y || 0}px)`,
          width: `${blob.size + (mouseInfluence[index]?.sizeChange || 0)}px`,
          height: `${blob.size + (mouseInfluence[index]?.sizeChange || 0)}px`,
          transform: `translate(-50%, -50%) scale(${blob.scale + (mouseInfluence[index]?.scale || 0)}) rotate(${mouseInfluence[index]?.rotation || 0}deg)`,
          opacity: blob.opacity + (mouseInfluence[index]?.opacityChange || 0),
          filter: `blur(${blob.blur + (mouseInfluence[index]?.blurChange || 0)}px)`,
          background: blob.gradient,
          animation: `${blob.animation} ${blob.duration}s ease-in-out infinite`,
          transition: 'left 0.1s ease-out, top 0.1s ease-out, transform 0.1s ease-out, width 0.1s ease-out, height 0.1s ease-out, opacity 0.2s ease-out, filter 0.2s ease-out'
        }"
      ></div>

      <!-- Additional morphing layer -->
      <div
        class="absolute inset-0 opacity-30 mix-blend-soft-light will-change-transform pointer-events-none"
        :style="{
          background: overlayGradient,
          transform: `translate(${mouseOffset.x * 0.12}px, ${mouseOffset.y * 0.12}px) scale(${1 + mouseMovementIntensity * 0.05})`,
          transition: 'transform 0.15s ease-out, background 1s ease-out'
        }"
      ></div>

      <!-- Mouse cursor glow effect - larger and more visible -->
      <div
        v-if="mousePos.x > 0"
        class="absolute rounded-full mix-blend-screen pointer-events-none will-change-transform"
        :style="{
          left: `${mousePos.x}px`,
          top: `${mousePos.y}px`,
          width: '600px',
          height: '600px',
          transform: `translate(-50%, -50%) scale(${1 + mouseMovementIntensity * 0.3})`,
          background: 'radial-gradient(circle, rgba(229, 231, 187, 0.4) 0%, rgba(209, 213, 219, 0.3) 40%, transparent 70%)',
          filter: 'blur(80px)',
          opacity: cursorIntensity,
          transition: 'transform 0.2s ease-out, opacity 0.3s ease-out'
        }"
      ></div>

      <!-- Secondary cursor ripple effect -->
      <div
        v-if="mousePos.x > 0"
        class="absolute rounded-full mix-blend-multiply pointer-events-none will-change-transform"
        :style="{
          left: `${mousePos.x}px`,
          top: `${mousePos.y}px`,
          width: '450px',
          height: '450px',
          transform: `translate(-50%, -50%) scale(${1.2 + mouseMovementIntensity * 0.4})`,
          background: 'radial-gradient(circle, rgba(209, 213, 219, 0.5) 0%, transparent 60%)',
          filter: 'blur(70px)',
          opacity: cursorIntensity * 0.8,
          transition: 'transform 0.15s ease-out, opacity 0.25s ease-out'
        }"
      ></div>
    </div>

    <!-- Content -->
    <div class="relative z-10 w-full pointer-events-none">
      <div class="container mx-auto px-6 py-20">
        <div class="max-w-4xl mx-auto space-y-32">
          <!-- Section 1 -->
          <section class="min-h-screen flex items-center justify-center">
            <div class="text-center space-y-6 pointer-events-auto">
              <h1 class="text-6xl md:text-8xl font-bold text-gray-800 tracking-tight">
                مرحله اول
              </h1>
              <p class="text-xl md:text-2xl text-gray-600 max-w-2xl mx-auto">
                حالت‌های بزرگ و آرام با حرکت ملایم
              </p>
            </div>
          </section>

          <!-- Section 2 -->
          <section class="min-h-screen flex items-center justify-center">
            <div class="text-center space-y-6 pointer-events-auto">
              <h1 class="text-6xl md:text-8xl font-bold text-gray-800 tracking-tight">
                مرحله دوم
              </h1>
              <p class="text-xl md:text-2xl text-gray-600 max-w-2xl mx-auto">
                حالت‌های کوچک و سریع با انرژی بالا
              </p>
            </div>
          </section>

          <!-- Section 3 -->
          <section class="min-h-screen flex items-center justify-center">
            <div class="text-center space-y-6 pointer-events-auto">
              <h1 class="text-6xl md:text-8xl font-bold text-gray-800 tracking-tight">
                مرحله سوم
              </h1>
              <p class="text-xl md:text-2xl text-gray-600 max-w-2xl mx-auto">
                موج‌های عمودی با حرکت ریتمیک
              </p>
            </div>
          </section>

          <!-- Section 4 -->
          <section class="min-h-screen flex items-center justify-center">
            <div class="text-center space-y-6 pointer-events-auto">
              <h1 class="text-6xl md:text-8xl font-bold text-gray-800 tracking-tight">
                مرحله چهارم
              </h1>
              <p class="text-xl md:text-2xl text-gray-600 max-w-2xl mx-auto">
                انفجار رنگ با پراکندگی دینامیک
              </p>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const gradientContainer = ref(null)
const mouseOffset = ref({ x: 0, y: 0 })
const mousePos = ref({ x: 0, y: 0 })
const prevMousePos = ref({ x: 0, y: 0 })
const mouseVelocity = ref({ x: 0, y: 0 })
const mouseMovementIntensity = ref(0)
const scrollProgress = ref(0)
const scrollStage = ref(0)
const mouseInfluence = ref([])
const cursorIntensity = ref(0)

// Stage configurations
const stageConfigs = [
  // Stage 1: Large calm blobs
  {
    turbulence: '0.01 0.01',
    turbulenceAnim: '0.01 0.01; 0.015 0.01; 0.01 0.01',
    displacement: 30,
    blur: 80,
    blobs: [
      { x: 20, y: 30, size: 600, speed: 0.4, gradient: 'radial-gradient(circle, #d1d5db 0%, #e5e7eb 50%, transparent 70%)', blur: 80, opacity: 0.4, scale: 1, duration: 15, animation: 'float1', repelStrength: 200, attractStrength: 80, blendMode: 'mix-blend-multiply' },
      { x: 70, y: 60, size: 500, speed: 0.35, gradient: 'radial-gradient(circle, #e5e7bb 0%, #d1d5db 50%, transparent 70%)', blur: 90, opacity: 0.35, scale: 1, duration: 18, animation: 'float2', repelStrength: 180, attractStrength: 70, blendMode: 'mix-blend-multiply' },
      { x: 50, y: 80, size: 550, speed: 0.3, gradient: 'radial-gradient(circle, #d1d5db 0%, #f3f4f6 50%, transparent 70%)', blur: 85, opacity: 0.3, scale: 1, duration: 20, animation: 'float3', repelStrength: 190, attractStrength: 75, blendMode: 'mix-blend-multiply' },
      { x: 35, y: 50, size: 480, speed: 0.38, gradient: 'radial-gradient(circle, #e5e7bb 0%, #e5e7eb 50%, transparent 70%)', blur: 88, opacity: 0.32, scale: 1, duration: 17, animation: 'float1', repelStrength: 185, attractStrength: 72, blendMode: 'mix-blend-multiply' },
    ]
  },
  // Stage 2: Small fast blobs
  {
    turbulence: '0.03 0.03',
    turbulenceAnim: '0.03 0.03; 0.05 0.04; 0.03 0.03',
    displacement: 50,
    blur: 40,
    blobs: [
      { x: 15, y: 25, size: 250, speed: 0.7, gradient: 'radial-gradient(circle, #d1d5db 0%, transparent 60%)', blur: 40, opacity: 0.5, scale: 1.1, duration: 6, animation: 'pulse1', repelStrength: 140, attractStrength: 60, blendMode: 'mix-blend-multiply' },
      { x: 45, y: 40, size: 200, speed: 0.8, gradient: 'radial-gradient(circle, #e5e7bb 0%, transparent 60%)', blur: 35, opacity: 0.45, scale: 1.15, duration: 5, animation: 'pulse2', repelStrength: 130, attractStrength: 55, blendMode: 'mix-blend-multiply' },
      { x: 75, y: 55, size: 220, speed: 0.75, gradient: 'radial-gradient(circle, #d1d5db 0%, transparent 60%)', blur: 38, opacity: 0.48, scale: 1.12, duration: 5.5, animation: 'pulse3', repelStrength: 135, attractStrength: 58, blendMode: 'mix-blend-multiply' },
      { x: 30, y: 70, size: 180, speed: 0.85, gradient: 'radial-gradient(circle, #e5e7bb 0%, transparent 60%)', blur: 42, opacity: 0.42, scale: 1.08, duration: 4.5, animation: 'pulse1', repelStrength: 125, attractStrength: 52, blendMode: 'mix-blend-multiply' },
      { x: 60, y: 85, size: 210, speed: 0.72, gradient: 'radial-gradient(circle, #d1d5db 0%, transparent 60%)', blur: 36, opacity: 0.46, scale: 1.13, duration: 6.5, animation: 'pulse2', repelStrength: 132, attractStrength: 56, blendMode: 'mix-blend-multiply' },
      { x: 85, y: 35, size: 190, speed: 0.78, gradient: 'radial-gradient(circle, #e5e7bb 0%, transparent 60%)', blur: 39, opacity: 0.44, scale: 1.1, duration: 5.8, animation: 'pulse3', repelStrength: 128, attractStrength: 54, blendMode: 'mix-blend-multiply' },
      { x: 50, y: 15, size: 170, speed: 0.82, gradient: 'radial-gradient(circle, #d1d5db 0%, transparent 60%)', blur: 41, opacity: 0.47, scale: 1.09, duration: 5.2, animation: 'pulse1', repelStrength: 127, attractStrength: 53, blendMode: 'mix-blend-multiply' },
    ]
  },
  // Stage 3: Vertical waves
  {
    turbulence: '0.02 0.04',
    turbulenceAnim: '0.02 0.04; 0.025 0.06; 0.02 0.04',
    displacement: 40,
    blur: 60,
    blobs: [
      { x: 20, y: 20, size: 300, speed: 0.5, gradient: 'linear-gradient(180deg, #d1d5db 0%, #e5e7bb 50%, transparent 100%)', blur: 60, opacity: 0.4, scale: 1, duration: 10, animation: 'wave1', repelStrength: 160, attractStrength: 65, blendMode: 'mix-blend-multiply' },
      { x: 40, y: 40, size: 350, speed: 0.45, gradient: 'linear-gradient(180deg, #e5e7bb 0%, #d1d5db 50%, transparent 100%)', blur: 65, opacity: 0.38, scale: 1, duration: 12, animation: 'wave2', repelStrength: 170, attractStrength: 68, blendMode: 'mix-blend-multiply' },
      { x: 60, y: 30, size: 320, speed: 0.48, gradient: 'linear-gradient(180deg, #d1d5db 0%, #f3f4f6 50%, transparent 100%)', blur: 62, opacity: 0.42, scale: 1, duration: 11, animation: 'wave3', repelStrength: 165, attractStrength: 66, blendMode: 'mix-blend-multiply' },
      { x: 80, y: 50, size: 340, speed: 0.43, gradient: 'linear-gradient(180deg, #e5e7bb 0%, #d1d5db 50%, transparent 100%)', blur: 58, opacity: 0.36, scale: 1, duration: 13, animation: 'wave1', repelStrength: 168, attractStrength: 67, blendMode: 'mix-blend-multiply' },
      { x: 30, y: 70, size: 310, speed: 0.47, gradient: 'linear-gradient(180deg, #d1d5db 0%, #e5e7bb 50%, transparent 100%)', blur: 61, opacity: 0.39, scale: 1, duration: 11.5, animation: 'wave2', repelStrength: 163, attractStrength: 65, blendMode: 'mix-blend-multiply' },
    ]
  },
  // Stage 4: Explosive scattered
  {
    turbulence: '0.04 0.04',
    turbulenceAnim: '0.04 0.04; 0.06 0.05; 0.04 0.04',
    displacement: 60,
    blur: 50,
    blobs: [
      { x: 10, y: 15, size: 280, speed: 0.9, gradient: 'radial-gradient(circle, #d1d5db 0%, #e5e7bb 40%, transparent 70%)', blur: 50, opacity: 0.5, scale: 1.2, duration: 4, animation: 'explode1', repelStrength: 220, attractStrength: 85, blendMode: 'mix-blend-multiply' },
      { x: 30, y: 30, size: 240, speed: 1.0, gradient: 'radial-gradient(circle, #e5e7bb 0%, #d1d5db 40%, transparent 70%)', blur: 45, opacity: 0.48, scale: 1.25, duration: 3.5, animation: 'explode2', repelStrength: 210, attractStrength: 82, blendMode: 'mix-blend-multiply' },
      { x: 50, y: 45, size: 260, speed: 0.95, gradient: 'radial-gradient(circle, #d1d5db 0%, transparent 70%)', blur: 48, opacity: 0.52, scale: 1.22, duration: 4.2, animation: 'explode3', repelStrength: 215, attractStrength: 83, blendMode: 'mix-blend-multiply' },
      { x: 70, y: 60, size: 220, speed: 1.05, gradient: 'radial-gradient(circle, #e5e7bb 0%, transparent 70%)', blur: 52, opacity: 0.46, scale: 1.18, duration: 3.8, animation: 'explode1', repelStrength: 205, attractStrength: 80, blendMode: 'mix-blend-multiply' },
      { x: 90, y: 75, size: 250, speed: 0.92, gradient: 'radial-gradient(circle, #d1d5db 0%, #e5e7bb 40%, transparent 70%)', blur: 46, opacity: 0.49, scale: 1.24, duration: 4.5, animation: 'explode2', repelStrength: 212, attractStrength: 81, blendMode: 'mix-blend-multiply' },
      { x: 40, y: 85, size: 230, speed: 0.98, gradient: 'radial-gradient(circle, #e5e7bb 0%, transparent 70%)', blur: 54, opacity: 0.44, scale: 1.21, duration: 3.6, animation: 'explode3', repelStrength: 208, attractStrength: 79, blendMode: 'mix-blend-multiply' },
      { x: 65, y: 20, size: 245, speed: 0.93, gradient: 'radial-gradient(circle, #d1d5db 0%, #e5e7bb 40%, transparent 70%)', blur: 49, opacity: 0.47, scale: 1.23, duration: 4.1, animation: 'explode1', repelStrength: 213, attractStrength: 82, blendMode: 'mix-blend-multiply' },
      { x: 20, y: 50, size: 235, speed: 0.97, gradient: 'radial-gradient(circle, #e5e7bb 0%, #d1d5db 40%, transparent 70%)', blur: 51, opacity: 0.45, scale: 1.19, duration: 3.9, animation: 'explode2', repelStrength: 209, attractStrength: 80, blendMode: 'mix-blend-multiply' },
    ]
  }
]

const turbulenceFrequency = computed(() => stageConfigs[scrollStage.value].turbulence)
const turbulenceAnimation = computed(() => stageConfigs[scrollStage.value].turbulenceAnim)
const displacementScale = computed(() => stageConfigs[scrollStage.value].displacement)
const blurAmount = computed(() => stageConfigs[scrollStage.value].blur)
const blobs = computed(() => stageConfigs[scrollStage.value].blobs)

const overlayGradient = computed(() => {
  const stage = scrollStage.value
  const gradients = [
    'radial-gradient(circle at 30% 40%, #d1d5db 0%, transparent 50%)',
    'radial-gradient(circle at 60% 50%, #e5e7bb 0%, transparent 40%)',
    'linear-gradient(180deg, #d1d5db 0%, transparent 60%)',
    'radial-gradient(ellipse at 50% 50%, #e5e7bb 0%, transparent 50%)'
  ]
  return gradients[stage]
})

const calculateBlobInfluence = () => {
  const currentBlobs = stageConfigs[scrollStage.value].blobs
  mouseInfluence.value = currentBlobs.map((blob) => {
    // Calculate blob position in pixels
    const blobX = (blob.x / 100) * window.innerWidth
    const blobY = (blob.y / 100) * window.innerHeight
    
    // Calculate distance from mouse to blob center
    const dx = mousePos.value.x - blobX
    const dy = mousePos.value.y - blobY
    const distance = Math.sqrt(dx * dx + dy * dy)
    
    // Calculate repulsion and attraction forces
    const maxDistance = 500 // Maximum distance for influence
    const repelStrength = blob.repelStrength || 150
    const attractStrength = blob.attractStrength || 60
    
    if (distance < maxDistance && distance > 0) {
      const normalizedDistance = distance / maxDistance
      
      // Repulsion force (stronger when very close)
      const repelForce = Math.pow(1 - normalizedDistance, 2) * repelStrength
      
      // Attraction force (pulls slightly at medium distance)
      const attractForce = normalizedDistance < 0.5 ? 0 : (normalizedDistance - 0.5) * 2 * attractStrength
      
      const totalForce = -(repelForce - attractForce)
      const angle = Math.atan2(dy, dx)
      
      // Add velocity influence
      const velocityInfluence = Math.sqrt(mouseVelocity.value.x ** 2 + mouseVelocity.value.y ** 2) * 0.3
      
      return {
        x: Math.cos(angle) * totalForce * (1 + velocityInfluence),
        y: Math.sin(angle) * totalForce * (1 + velocityInfluence),
        scale: (1 - normalizedDistance) * 0.25,
        rotation: (1 - normalizedDistance) * 15 * (Math.random() > 0.5 ? 1 : -1),
        opacityChange: (1 - normalizedDistance) * 0.15,
        blurChange: -(1 - normalizedDistance) * 20,
        sizeChange: (1 - normalizedDistance) * 50
      }
    }
    
    return { x: 0, y: 0, scale: 0, rotation: 0, opacityChange: 0, blurChange: 0, sizeChange: 0 }
  })
}

const handleMouseMove = (e) => {
  // Calculate velocity
  const velocityX = e.clientX - prevMousePos.value.x
  const velocityY = e.clientY - prevMousePos.value.y
  mouseVelocity.value = { x: velocityX, y: velocityY }
  
  // Calculate movement intensity
  const speed = Math.sqrt(velocityX ** 2 + velocityY ** 2)
  mouseMovementIntensity.value = Math.min(speed / 50, 1)
  
  prevMousePos.value = { x: e.clientX, y: e.clientY }
  mousePos.value = { x: e.clientX, y: e.clientY }
  
  const x = (e.clientX / window.innerWidth - 0.5) * 150
  const y = (e.clientY / window.innerHeight - 0.5) * 150
  mouseOffset.value = { x, y }
  
  cursorIntensity.value = 1
  
  calculateBlobInfluence()
}

const handleMouseLeave = () => {
  cursorIntensity.value = 0
  mouseMovementIntensity.value = 0
}

const handleScroll = () => {
  const scrollHeight = document.documentElement.scrollHeight - window.innerHeight
  const currentScroll = window.scrollY
  scrollProgress.value = currentScroll / scrollHeight
  
  // Calculate stage based on scroll (4 stages)
  if (scrollProgress.value < 0.25) {
    scrollStage.value = 0
  } else if (scrollProgress.value < 0.5) {
    scrollStage.value = 1
  } else if (scrollProgress.value < 0.75) {
    scrollStage.value = 2
  } else {
    scrollStage.value = 3
  }
}

let rafId = null
const animate = () => {
  // Gradually decrease movement intensity
  mouseMovementIntensity.value *= 0.95
  
  // Gradually decrease velocity
  mouseVelocity.value.x *= 0.9
  mouseVelocity.value.y *= 0.9
  
  rafId = requestAnimationFrame(animate)
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove, { passive: true })
  window.addEventListener('mouseleave', handleMouseLeave, { passive: true })
  window.addEventListener('scroll', handleScroll, { passive: true })
  handleScroll()
  animate()
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('mouseleave', handleMouseLeave)
  window.removeEventListener('scroll', handleScroll)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>

<style>
@keyframes float1 {
  0%, 100% { transform: translate(-50%, -50%) translateY(0px) rotate(0deg); }
  33% { transform: translate(-50%, -50%) translateY(-30px) rotate(5deg); }
  66% { transform: translate(-50%, -50%) translateY(20px) rotate(-5deg); }
}

@keyframes float2 {
  0%, 100% { transform: translate(-50%, -50%) translateX(0px) rotate(0deg); }
  33% { transform: translate(-50%, -50%) translateX(25px) rotate(-5deg); }
  66% { transform: translate(-50%, -50%) translateX(-25px) rotate(5deg); }
}

@keyframes float3 {
  0%, 100% { transform: translate(-50%, -50%) translate(0px, 0px) rotate(0deg); }
  25% { transform: translate(-50%, -50%) translate(20px, -20px) rotate(3deg); }
  50% { transform: translate(-50%, -50%) translate(-15px, 15px) rotate(-3deg); }
  75% { transform: translate(-50%, -50%) translate(15px, 20px) rotate(2deg); }
}

@keyframes pulse1 {
  0%, 100% { transform: translate(-50%, -50%) scale(1); }
  50% { transform: translate(-50%, -50%) scale(1.3); }
}

@keyframes pulse2 {
  0%, 100% { transform: translate(-50%, -50%) scale(1) rotate(0deg); }
  50% { transform: translate(-50%, -50%) scale(1.35) rotate(10deg); }
}

@keyframes pulse3 {
  0%, 100% { transform: translate(-50%, -50%) scale(1); }
  33% { transform: translate(-50%, -50%) scale(1.25); }
  66% { transform: translate(-50%, -50%) scale(1.1); }
}

@keyframes wave1 {
  0%, 100% { transform: translate(-50%, -50%) translateY(0px) scaleY(1); }
  50% { transform: translate(-50%, -50%) translateY(-40px) scaleY(1.2); }
}

@keyframes wave2 {
  0%, 100% { transform: translate(-50%, -50%) translateY(0px) scaleY(1); }
  50% { transform: translate(-50%, -50%) translateY(40px) scaleY(1.15); }
}

@keyframes wave3 {
  0%, 100% { transform: translate(-50%, -50%) translateY(0px) scaleY(1) scaleX(1); }
  33% { transform: translate(-50%, -50%) translateY(-30px) scaleY(1.1) scaleX(0.95); }
  66% { transform: translate(-50%, -50%) translateY(30px) scaleY(1.15) scaleX(1.05); }
}

@keyframes explode1 {
  0%, 100% { transform: translate(-50%, -50%) scale(1) rotate(0deg); }
  25% { transform: translate(-50%, -50%) scale(1.4) rotate(20deg); }
  50% { transform: translate(-50%, -50%) scale(0.9) rotate(-15deg); }
  75% { transform: translate(-50%, -50%) scale(1.3) rotate(25deg); }
}

@keyframes explode2 {
  0%, 100% { transform: translate(-50%, -50%) scale(1) translate(0, 0); }
  33% { transform: translate(-50%, -50%) scale(1.5) translate(30px, -30px); }
  66% { transform: translate(-50%, -50%) scale(0.8) translate(-30px, 30px); }
}

@keyframes explode3 {
  0%, 100% { transform: translate(-50%, -50%) scale(1) rotate(0deg); }
  20% { transform: translate(-50%, -50%) scale(1.35) rotate(30deg); }
  40% { transform: translate(-50%, -50%) scale(0.85) rotate(-20deg); }
  60% { transform: translate(-50%, -50%) scale(1.4) rotate(25deg); }
  80% { transform: translate(-50%, -50%) scale(0.9) rotate(-15deg); }
}
</style>