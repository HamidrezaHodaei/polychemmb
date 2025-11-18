<template>
  <div class="w-full h-screen bg-black overflow-hidden relative">
    <canvas ref="globeCanvas" class="w-full h-full"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const globeCanvas = ref(null)

onMounted(() => {
  if (!globeCanvas.value) return

  const canvas = globeCanvas.value
  const ctx = canvas.getContext('2d')
  
  let width = canvas.width = window.innerWidth
  let height = canvas.height = window.innerHeight
  
  // Mouse tracking
  let isDragging = false
  let lastMouseX = 0
  let lastMouseY = 0
  
  // Globe rotation
  let rotationX = 0.3
  let rotationY = 0
  let targetRotationX = 0.3
  let targetRotationY = 0
  let velocityX = 0.002
  let velocityY = 0
  
  // Globe settings
  const radius = Math.min(width, height) * 0.35
  const centerX = width / 2
  const centerY = height / 2
  
  // داده‌های قاره‌ها - مختصات واقعی با دقت بالا
  const continentData = [
    // آمریکای شمالی
    { lat: 40, lon: -100, size: 1 },
    { lat: 45, lon: -95, size: 1 },
    { lat: 50, lon: -100, size: 1 },
    { lat: 55, lon: -105, size: 1 },
    { lat: 60, lon: -110, size: 1 },
    { lat: 65, lon: -115, size: 1 },
    { lat: 70, lon: -105, size: 1 },
    { lat: 35, lon: -105, size: 1 },
    { lat: 30, lon: -110, size: 1 },
    { lat: 25, lon: -100, size: 1 },
    { lat: 40, lon: -75, size: 1 },
    { lat: 45, lon: -80, size: 1 },
    { lat: 50, lon: -85, size: 1 },
    { lat: 35, lon: -85, size: 1 },
    { lat: 30, lon: -90, size: 1 },
    { lat: 25, lon: -80, size: 1 },
    { lat: 50, lon: -120, size: 1 },
    { lat: 55, lon: -125, size: 1 },
    { lat: 60, lon: -130, size: 1 },
    { lat: 40, lon: -120, size: 1 },
    
    // آمریکای جنوبی
    { lat: -10, lon: -60, size: 1 },
    { lat: -15, lon: -55, size: 1 },
    { lat: -20, lon: -60, size: 1 },
    { lat: -25, lon: -65, size: 1 },
    { lat: -30, lon: -65, size: 1 },
    { lat: -35, lon: -65, size: 1 },
    { lat: -40, lon: -70, size: 1 },
    { lat: -45, lon: -70, size: 1 },
    { lat: 0, lon: -60, size: 1 },
    { lat: -5, lon: -65, size: 1 },
    { lat: -10, lon: -70, size: 1 },
    { lat: 5, lon: -70, size: 1 },
    { lat: 0, lon: -75, size: 1 },
    
    // اروپا
    { lat: 50, lon: 10, size: 1 },
    { lat: 55, lon: 15, size: 1 },
    { lat: 60, lon: 20, size: 1 },
    { lat: 65, lon: 25, size: 1 },
    { lat: 45, lon: 5, size: 1 },
    { lat: 50, lon: 0, size: 1 },
    { lat: 40, lon: -5, size: 1 },
    { lat: 55, lon: -5, size: 1 },
    { lat: 60, lon: 10, size: 1 },
    { lat: 50, lon: 20, size: 1 },
    { lat: 45, lon: 15, size: 1 },
    { lat: 40, lon: 15, size: 1 },
    
    // آفریقا
    { lat: 10, lon: 20, size: 1 },
    { lat: 5, lon: 25, size: 1 },
    { lat: 0, lon: 30, size: 1 },
    { lat: -5, lon: 30, size: 1 },
    { lat: -10, lon: 30, size: 1 },
    { lat: -15, lon: 30, size: 1 },
    { lat: -20, lon: 25, size: 1 },
    { lat: -25, lon: 25, size: 1 },
    { lat: -30, lon: 25, size: 1 },
    { lat: 15, lon: 15, size: 1 },
    { lat: 20, lon: 10, size: 1 },
    { lat: 25, lon: 15, size: 1 },
    { lat: 30, lon: 20, size: 1 },
    { lat: 0, lon: 15, size: 1 },
    { lat: -10, lon: 20, size: 1 },
    { lat: 10, lon: 35, size: 1 },
    { lat: 5, lon: 40, size: 1 },
    { lat: -20, lon: 35, size: 1 },
    { lat: -25, lon: 30, size: 1 },
    
    // آسیا
    { lat: 60, lon: 100, size: 1 },
    { lat: 65, lon: 90, size: 1 },
    { lat: 70, lon: 80, size: 1 },
    { lat: 50, lon: 80, size: 1 },
    { lat: 45, lon: 90, size: 1 },
    { lat: 40, lon: 100, size: 1 },
    { lat: 35, lon: 110, size: 1 },
    { lat: 30, lon: 120, size: 1 },
    { lat: 25, lon: 110, size: 1 },
    { lat: 20, lon: 100, size: 1 },
    { lat: 50, lon: 60, size: 1 },
    { lat: 45, lon: 70, size: 1 },
    { lat: 55, lon: 50, size: 1 },
    { lat: 60, lon: 60, size: 1 },
    { lat: 40, lon: 50, size: 1 },
    { lat: 35, lon: 60, size: 1 },
    { lat: 30, lon: 70, size: 1 },
    { lat: 25, lon: 80, size: 1 },
    { lat: 40, lon: 130, size: 1 },
    { lat: 35, lon: 140, size: 1 },
    
    // استرالیا
    { lat: -25, lon: 130, size: 1 },
    { lat: -30, lon: 135, size: 1 },
    { lat: -35, lon: 140, size: 1 },
    { lat: -20, lon: 125, size: 1 },
    { lat: -25, lon: 145, size: 1 },
    { lat: -30, lon: 150, size: 1 },
    { lat: -35, lon: 145, size: 1 },
    { lat: -40, lon: 145, size: 1 },
  ]
  
  // تولید نقاط بیشتر برای هر قاره
  const dots = []
  continentData.forEach(point => {
    // اضافه کردن نقاط اطراف هر نقطه اصلی
    for (let dLat = -8; dLat <= 8; dLat += 2) {
      for (let dLon = -8; dLon <= 8; dLon += 2) {
        const lat = point.lat + dLat
        const lon = point.lon + dLon
        const theta = ((90 - lat) / 180) * Math.PI
        const phi = ((lon + 180) / 360) * Math.PI * 2
        
        dots.push({
          theta,
          phi,
          brightness: 1
        })
      }
    }
  })
  
  function project3DTo2D(theta, phi, rotX, rotY) {
    let x = Math.sin(theta) * Math.cos(phi)
    let y = Math.cos(theta)
    let z = Math.sin(theta) * Math.sin(phi)
    
    const cosY = Math.cos(rotY)
    const sinY = Math.sin(rotY)
    const tempX = x * cosY - z * sinY
    const tempZ = x * sinY + z * cosY
    x = tempX
    z = tempZ
    
    const cosX = Math.cos(rotX)
    const sinX = Math.sin(rotX)
    const tempY = y * cosX - z * sinX
    z = y * sinX + z * cosX
    y = tempY
    
    const scale = radius / (radius + z * 0.5)
    const x2d = centerX + x * radius * scale
    const y2d = centerY + y * radius * scale
    
    return { x: x2d, y: y2d, z, scale, visible: z > -radius * 0.3 }
  }
  
  function drawGlobe() {
    ctx.fillStyle = '#000000'
    ctx.fillRect(0, 0, width, height)
    
    const allPoints = []
    
    dots.forEach(dot => {
      const pos = project3DTo2D(dot.theta, dot.phi, rotationX, rotationY)
      if (pos.visible) {
        allPoints.push({
          x: pos.x,
          y: pos.y,
          z: pos.z,
          brightness: dot.brightness * pos.scale,
          size: 2 * pos.scale
        })
      }
    })
    
    allPoints.sort((a, b) => a.z - b.z)
    
    allPoints.forEach(point => {
      const alpha = Math.max(0, Math.min(1, point.brightness))
      ctx.fillStyle = `rgba(255, 255, 255, ${alpha})`
      ctx.beginPath()
      ctx.arc(point.x, point.y, point.size, 0, Math.PI * 2)
      ctx.fill()
    })
    
    // درخشش لبه
    const gradient = ctx.createRadialGradient(centerX, centerY, radius * 0.85, centerX, centerY, radius * 1.05)
    gradient.addColorStop(0, 'rgba(255, 255, 255, 0)')
    gradient.addColorStop(0.85, 'rgba(255, 255, 255, 0.1)')
    gradient.addColorStop(1, 'rgba(255, 255, 255, 0)')
    ctx.fillStyle = gradient
    ctx.beginPath()
    ctx.arc(centerX, centerY, radius * 1.05, 0, Math.PI * 2)
    ctx.fill()
  }
  
  function animate() {
    rotationX += (targetRotationX - rotationX) * 0.1
    rotationY += (targetRotationY - rotationY) * 0.1
    
    if (!isDragging) {
      targetRotationY += velocityX
      targetRotationX += velocityY
      velocityX *= 0.95
      velocityY *= 0.95
    }
    
    drawGlobe()
    requestAnimationFrame(animate)
  }
  
  function handleMouseDown(e) {
    isDragging = true
    lastMouseX = e.clientX
    lastMouseY = e.clientY
    velocityX = 0
    velocityY = 0
  }
  
  function handleMouseMove(e) {
    if (isDragging) {
      const deltaX = e.clientX - lastMouseX
      const deltaY = e.clientY - lastMouseY
      
      velocityX = deltaX * 0.005
      velocityY = deltaY * 0.005
      
      targetRotationY += deltaX * 0.005
      targetRotationX += deltaY * 0.005
      
      lastMouseX = e.clientX
      lastMouseY = e.clientY
    }
  }
  
  function handleMouseUp() {
    isDragging = false
  }
  
  function handleResize() {
    width = canvas.width = window.innerWidth
    height = canvas.height = window.innerHeight
  }
  
  function handleTouchStart(e) {
    e.preventDefault()
    const touch = e.touches[0]
    handleMouseDown({ clientX: touch.clientX, clientY: touch.clientY })
  }
  
  function handleTouchMove(e) {
    e.preventDefault()
    const touch = e.touches[0]
    handleMouseMove({ clientX: touch.clientX, clientY: touch.clientY })
  }
  
  function handleTouchEnd(e) {
    e.preventDefault()
    handleMouseUp()
  }
  
  canvas.addEventListener('mousedown', handleMouseDown)
  canvas.addEventListener('mousemove', handleMouseMove)
  canvas.addEventListener('mouseup', handleMouseUp)
  canvas.addEventListener('mouseleave', handleMouseUp)
  canvas.addEventListener('touchstart', handleTouchStart, { passive: false })
  canvas.addEventListener('touchmove', handleTouchMove, { passive: false })
  canvas.addEventListener('touchend', handleTouchEnd, { passive: false })
  window.addEventListener('resize', handleResize)
  
  animate()
  
  onUnmounted(() => {
    canvas.removeEventListener('mousedown', handleMouseDown)
    canvas.removeEventListener('mousemove', handleMouseMove)
    canvas.removeEventListener('mouseup', handleMouseUp)
    canvas.removeEventListener('mouseleave', handleMouseUp)
    canvas.removeEventListener('touchstart', handleTouchStart)
    canvas.removeEventListener('touchmove', handleTouchMove)
    canvas.removeEventListener('touchend', handleTouchEnd)
    window.removeEventListener('resize', handleResize)
  })
})
</script>

<style scoped>
canvas {
  cursor: grab;
}

canvas:active {
  cursor: grabbing;
}
</style>