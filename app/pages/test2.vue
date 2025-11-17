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
  let rotationX = 0.2
  let rotationY = 0
  let targetRotationX = 0.2
  let targetRotationY = 0
  let velocityX = 0.003
  let velocityY = 0
  
  // Globe settings
  const radius = Math.min(width, height) * 0.35
  const centerX = width / 2
  const centerY = height / 2
  
  // بارگذاری تصویر texture
  const earthTexture = new Image()
  earthTexture.crossOrigin = "anonymous"
  let textureLoaded = false
  let dots = []
  
  // استفاده از تصویر واقعی نقشه زمین (سیاه و سفید)
  earthTexture.src = 'https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Equirectangular_projection_SW.jpg/1280px-Equirectangular_projection_SW.jpg'
  
  earthTexture.onload = () => {
    textureLoaded = true
    generateDotsFromTexture()
  }
  
  // در صورت عدم بارگذاری، از texture دستی استفاده می‌کنیم
  earthTexture.onerror = () => {
    console.log('Using fallback texture')
    generateFallbackTexture()
  }
  
  function generateDotsFromTexture() {
    const tempCanvas = document.createElement('canvas')
    const tempCtx = tempCanvas.getContext('2d')
    
    const texWidth = 512
    const texHeight = 256
    tempCanvas.width = texWidth
    tempCanvas.height = texHeight
    
    // رسم تصویر روی canvas موقت
    tempCtx.drawImage(earthTexture, 0, 0, texWidth, texHeight)
    const imageData = tempCtx.getImageData(0, 0, texWidth, texHeight)
    
    dots = []
    const numLat = 100
    const numLon = 200
    
    for (let lat = 0; lat < numLat; lat++) {
      for (let lon = 0; lon < numLon; lon++) {
        const theta = (lat / numLat) * Math.PI
        const phi = (lon / numLon) * Math.PI * 2
        
        // خواندن رنگ پیکسل از texture
        const texX = Math.floor((lon / numLon) * texWidth)
        const texY = Math.floor((lat / numLat) * texHeight)
        const pixelIndex = (texY * texWidth + texX) * 4
        
        const r = imageData.data[pixelIndex]
        const g = imageData.data[pixelIndex + 1]
        const b = imageData.data[pixelIndex + 2]
        
        // محاسبه روشنایی
        const brightness = (r + g + b) / 3
        
        // فقط نقاط روشن (خشکی) را نگه می‌داریم
        if (brightness > 100) {
          dots.push({
            theta,
            phi,
            brightness: brightness / 255
          })
        }
      }
    }
  }
  
  function generateFallbackTexture() {
    // تولید texture ساده در صورت عدم بارگذاری تصویر
    const tempCanvas = document.createElement('canvas')
    const tempCtx = tempCanvas.getContext('2d')
    const texSize = 512
    tempCanvas.width = texSize
    tempCanvas.height = texSize / 2
    
    tempCtx.fillStyle = '#000000'
    tempCtx.fillRect(0, 0, texSize, texSize / 2)
    
    // رسم قاره‌های ساده
    tempCtx.fillStyle = '#FFFFFF'
    
    // آمریکا
    tempCtx.fillRect(50, 80, 120, 150)
    // اروپا و آسیا
    tempCtx.fillRect(200, 60, 280, 120)
    // آفریقا
    tempCtx.fillRect(220, 140, 80, 100)
    // استرالیا
    tempCtx.fillRect(420, 200, 60, 50)
    
    const imageData = tempCtx.getImageData(0, 0, texSize, texSize / 2)
    
    dots = []
    const numLat = 80
    const numLon = 160
    
    for (let lat = 0; lat < numLat; lat++) {
      for (let lon = 0; lon < numLon; lon++) {
        const theta = (lat / numLat) * Math.PI
        const phi = (lon / numLon) * Math.PI * 2
        
        const texX = Math.floor((lon / numLon) * texSize)
        const texY = Math.floor((lat / numLat) * (texSize / 2))
        const pixelIndex = (texY * texSize + texX) * 4
        
        const brightness = imageData.data[pixelIndex]
        
        if (brightness > 128) {
          dots.push({
            theta,
            phi,
            brightness: 1
          })
        }
      }
    }
  }
  
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
    
    if (dots.length === 0) return
    
    const allPoints = []
    
    dots.forEach(dot => {
      const pos = project3DTo2D(dot.theta, dot.phi, rotationX, rotationY)
      if (pos.visible) {
        allPoints.push({
          x: pos.x,
          y: pos.y,
          z: pos.z,
          brightness: dot.brightness * pos.scale,
          size: 2.5 * pos.scale
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
    gradient.addColorStop(0.85, 'rgba(255, 255, 255, 0.12)')
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
  
  // شروع با texture fallback تا زمان بارگذاری تصویر
  generateFallbackTexture()
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