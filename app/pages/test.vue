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
  let mouseX = 0
  let mouseY = 0
  let isDragging = false
  let lastMouseX = 0
  let lastMouseY = 0
  
  // Globe rotation
  let rotationX = 0
  let rotationY = 0
  let targetRotationX = 0
  let targetRotationY = 0
  let velocityX = 0
  let velocityY = 0
  
  // Globe settings
  const radius = Math.min(width, height) * 0.35
  const centerX = width / 2
  const centerY = height / 2
  
  // Create dots for globe surface
  const dots = []
  const numLat = 40
  const numLon = 80
  
  // Generate dots in a sphere pattern
  for (let lat = 0; lat < numLat; lat++) {
    for (let lon = 0; lon < numLon; lon++) {
      const theta = (lat / numLat) * Math.PI
      const phi = (lon / numLon) * Math.PI * 2
      
      dots.push({
        theta,
        phi,
        baseBrightness: Math.random() * 0.3 + 0.7
      })
    }
  }
  
  // Add random bright spots (cities/lights)
  const brightSpots = []
  for (let i = 0; i < 150; i++) {
    brightSpots.push({
      theta: Math.random() * Math.PI,
      phi: Math.random() * Math.PI * 2,
      brightness: Math.random() * 0.5 + 0.5,
      size: Math.random() * 2 + 1
    })
  }
  
  function project3DTo2D(theta, phi, rotX, rotY) {
    // Convert spherical to Cartesian
    let x = Math.sin(theta) * Math.cos(phi)
    let y = Math.cos(theta)
    let z = Math.sin(theta) * Math.sin(phi)
    
    // Rotate around Y axis
    const cosY = Math.cos(rotY)
    const sinY = Math.sin(rotY)
    const tempX = x * cosY - z * sinY
    const tempZ = x * sinY + z * cosY
    x = tempX
    z = tempZ
    
    // Rotate around X axis
    const cosX = Math.cos(rotX)
    const sinX = Math.sin(rotX)
    const tempY = y * cosX - z * sinX
    z = y * sinX + z * cosX
    y = tempY
    
    // Project to 2D
    const scale = radius / (radius + z * 0.5)
    const x2d = centerX + x * radius * scale
    const y2d = centerY + y * radius * scale
    
    return { x: x2d, y: y2d, z, scale, visible: z > -radius * 0.3 }
  }
  
  function drawGlobe() {
    ctx.fillStyle = '#000000'
    ctx.fillRect(0, 0, width, height)
    
    // Draw all dots
    const allPoints = []
    
    // Regular grid dots
    dots.forEach(dot => {
      const pos = project3DTo2D(dot.theta, dot.phi, rotationX, rotationY)
      if (pos.visible) {
        allPoints.push({
          x: pos.x,
          y: pos.y,
          z: pos.z,
          brightness: dot.baseBrightness * pos.scale,
          size: 1.5 * pos.scale
        })
      }
    })
    
    // Bright spots (cities)
    brightSpots.forEach(spot => {
      const pos = project3DTo2D(spot.theta, spot.phi, rotationX, rotationY)
      if (pos.visible) {
        allPoints.push({
          x: pos.x,
          y: pos.y,
          z: pos.z,
          brightness: spot.brightness * pos.scale,
          size: spot.size * pos.scale,
          isCity: true
        })
      }
    })
    
    // Sort by z-depth
    allPoints.sort((a, b) => a.z - b.z)
    
    // Draw dots
    allPoints.forEach(point => {
      const alpha = Math.max(0, Math.min(1, point.brightness))
      const intensity = Math.floor(alpha * 255)
      
      if (point.isCity) {
        // Bright white spots for cities
        ctx.fillStyle = `rgba(255, 255, 255, ${alpha})`
        ctx.beginPath()
        ctx.arc(point.x, point.y, point.size, 0, Math.PI * 2)
        ctx.fill()
        
        // Glow effect
        const gradient = ctx.createRadialGradient(point.x, point.y, 0, point.x, point.y, point.size * 3)
        gradient.addColorStop(0, `rgba(138, 43, 226, ${alpha * 0.3})`)
        gradient.addColorStop(1, 'rgba(138, 43, 226, 0)')
        ctx.fillStyle = gradient
        ctx.beginPath()
        ctx.arc(point.x, point.y, point.size * 3, 0, Math.PI * 2)
        ctx.fill()
      } else {
        // Purple dots for grid
        ctx.fillStyle = `rgb(${intensity * 0.5}, ${intensity * 0.2}, ${intensity})`
        ctx.beginPath()
        ctx.arc(point.x, point.y, point.size, 0, Math.PI * 2)
        ctx.fill()
      }
    })
    
    // Draw globe outline/edge glow
    const gradient = ctx.createRadialGradient(centerX, centerY, radius * 0.8, centerX, centerY, radius * 1.1)
    gradient.addColorStop(0, 'rgba(138, 43, 226, 0)')
    gradient.addColorStop(0.8, 'rgba(138, 43, 226, 0.2)')
    gradient.addColorStop(1, 'rgba(138, 43, 226, 0)')
    ctx.fillStyle = gradient
    ctx.beginPath()
    ctx.arc(centerX, centerY, radius * 1.1, 0, Math.PI * 2)
    ctx.fill()
  }
  
  function animate() {
    // Smooth rotation interpolation
    rotationX += (targetRotationX - rotationX) * 0.1
    rotationY += (targetRotationY - rotationY) * 0.1
    
    // Add momentum/inertia
    if (!isDragging) {
      targetRotationY += velocityX
      targetRotationX += velocityY
      velocityX *= 0.95
      velocityY *= 0.95
    }
    
    drawGlobe()
    requestAnimationFrame(animate)
  }
  
  // Mouse event handlers
  function handleMouseDown(e) {
    isDragging = true
    lastMouseX = e.clientX
    lastMouseY = e.clientY
    velocityX = 0
    velocityY = 0
  }
  
  function handleMouseMove(e) {
    mouseX = e.clientX
    mouseY = e.clientY
    
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
  
  // Touch events for mobile
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
  
  // Add event listeners
  canvas.addEventListener('mousedown', handleMouseDown)
  canvas.addEventListener('mousemove', handleMouseMove)
  canvas.addEventListener('mouseup', handleMouseUp)
  canvas.addEventListener('mouseleave', handleMouseUp)
  canvas.addEventListener('touchstart', handleTouchStart, { passive: false })
  canvas.addEventListener('touchmove', handleTouchMove, { passive: false })
  canvas.addEventListener('touchend', handleTouchEnd, { passive: false })
  window.addEventListener('resize', handleResize)
  
  // Start animation
  animate()
  
  // Cleanup
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