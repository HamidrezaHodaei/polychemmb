<template>
  <div ref="containerRef" class="earth-container">
    <canvas ref="canvasRef"></canvas>
    <div v-if="selectedCountry" class="country-info">
      <h3>{{ selectedCountry.name }}</h3>
      <p>ISO: {{ selectedCountry.iso }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const props = defineProps({
  colorMap: {
    type: Object,
    default: () => ({})
  },
  autoRotate: {
    type: Boolean,
    default: true
  },
  rotationSpeed: {
    type: Number,
    default: 0.001
  }
})

const emit = defineEmits(['selectedCountry'])

const containerRef = ref(null)
const canvasRef = ref(null)
const selectedCountry = ref(null)

let scene, camera, renderer, controls, globe
let countries = []
let raycaster, mouse
let animationId
let isUserInteracting = false
let lastInteractionTime = Date.now()

// رنگ‌های پیش‌فرض برای قاره‌ها (سبک فلت)
const continentColors = {
  'AF': '#FFE5B4', // Africa - کرم روشن
  'AS': '#B4E5FF', // Asia - آبی روشن
  'EU': '#D4FFB4', // Europe - سبز روشن
  'NA': '#FFD4B4', // North America - نارنجی روشن
  'SA': '#FFB4D4', // South America - صورتی روشن
  'OC': '#E5B4FF', // Oceania - بنفش روشن
  'AN': '#FFFFFF'  // Antarctica - سفید
}

// دیتای ساده‌شده کشورها (GeoJSON ساده)
const worldData = {
  type: "FeatureCollection",
  features: [
    // نمونه کشورها - در production از فایل کامل استفاده کنید
    {
      type: "Feature",
      properties: { name: "United States", iso: "USA", continent: "NA" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [-125, 50], [-125, 25], [-65, 25], [-65, 50], [-125, 50]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Brazil", iso: "BRA", continent: "SA" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [-75, 5], [-75, -35], [-35, -35], [-35, 5], [-75, 5]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "China", iso: "CHN", continent: "AS" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [75, 55], [75, 20], [135, 20], [135, 55], [75, 55]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Russia", iso: "RUS", continent: "EU" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [20, 80], [20, 40], [180, 40], [180, 80], [20, 80]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Australia", iso: "AUS", continent: "OC" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [110, -10], [110, -45], [155, -45], [155, -10], [110, -10]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "India", iso: "IND", continent: "AS" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [68, 35], [68, 8], [97, 8], [97, 35], [68, 35]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Canada", iso: "CAN", continent: "NA" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [-140, 83], [-140, 42], [-52, 42], [-52, 83], [-140, 83]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Germany", iso: "DEU", continent: "EU" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [6, 55], [6, 47], [15, 47], [15, 55], [6, 55]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "France", iso: "FRA", continent: "EU" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [-5, 51], [-5, 42], [8, 42], [8, 51], [-5, 51]
        ]]
      }
    },
    {
      type: "Feature",
      properties: { name: "Japan", iso: "JPN", continent: "AS" },
      geometry: {
        type: "Polygon",
        coordinates: [[
          [129, 46], [129, 30], [146, 30], [146, 46], [129, 46]
        ]]
      }
    }
  ]
}

onMounted(() => {
  initScene()
  createGlobe()
  createCountries()
  animate()
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.addEventListener('resize', handleResize)
  if (animationId) cancelAnimationFrame(animationId)
  if (renderer) renderer.dispose()
})

watch(() => props.colorMap, () => {
  updateCountryColors()
}, { deep: true })

function initScene() {
  scene = new THREE.Scene()
  scene.background = new THREE.Color(0xFFFFFF) // پس‌زمینه سفید

  camera = new THREE.PerspectiveCamera(
    45,
    containerRef.value.clientWidth / containerRef.value.clientHeight,
    0.1,
    1000
  )
  camera.position.z = 3

  renderer = new THREE.WebGLRenderer({
    canvas: canvasRef.value,
    antialias: true,
    alpha: true
  })
  renderer.setSize(containerRef.value.clientWidth, containerRef.value.clientHeight)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

  // نورپردازی ساده
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.8)
  scene.add(ambientLight)

  const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5)
  directionalLight.position.set(5, 3, 5)
  scene.add(directionalLight)

  // OrbitControls
  controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true
  controls.dampingFactor = 0.05
  controls.minDistance = 1.5
  controls.maxDistance = 5
  controls.enablePan = true
  
  controls.addEventListener('start', () => {
    isUserInteracting = true
    lastInteractionTime = Date.now()
  })
  
  controls.addEventListener('end', () => {
    isUserInteracting = false
  })

  // Raycaster برای کلیک
  raycaster = new THREE.Raycaster()
  mouse = new THREE.Vector2()

  canvasRef.value.addEventListener('click', onCountryClick)
  canvasRef.value.addEventListener('touchend', onCountryClick)
}

function createGlobe() {
  // کره اصلی (اقیانوس‌ها)
  const geometry = new THREE.SphereGeometry(1, 64, 64)
  const material = new THREE.MeshPhongMaterial({
    color: 0xADD8E6, // آبی روشن
    flatShading: false,
    shininess: 5
  })
  
  globe = new THREE.Mesh(geometry, material)
  scene.add(globe)

  // خطوط شبکه (meridian & parallel)
  const gridMaterial = new THREE.LineBasicMaterial({ 
    color: 0xCCCCCC, 
    transparent: true, 
    opacity: 0.3 
  })
  
  for (let lat = -80; lat <= 80; lat += 20) {
    const points = []
    for (let lng = 0; lng <= 360; lng += 5) {
      const phi = (90 - lat) * (Math.PI / 180)
      const theta = (lng - 180) * (Math.PI / 180)
      const x = Math.sin(phi) * Math.cos(theta)
      const y = Math.cos(phi)
      const z = Math.sin(phi) * Math.sin(theta)
      points.push(new THREE.Vector3(x, y, z))
    }
    const gridGeometry = new THREE.BufferGeometry().setFromPoints(points)
    const gridLine = new THREE.Line(gridGeometry, gridMaterial)
    globe.add(gridLine)
  }
}

function latLngToVector3(lat, lng, radius = 1.01) {
  const phi = (90 - lat) * (Math.PI / 180)
  const theta = (lng + 180) * (Math.PI / 180)
  
  return new THREE.Vector3(
    -radius * Math.sin(phi) * Math.cos(theta),
    radius * Math.cos(phi),
    radius * Math.sin(phi) * Math.sin(theta)
  )
}

function createCountries() {
  worldData.features.forEach(feature => {
    const { name, iso, continent } = feature.properties
    const coords = feature.geometry.coordinates[0]
    
    // تبدیل مختصات به Vector3
    const shape = new THREE.Shape()
    coords.forEach((coord, i) => {
      const [lng, lat] = coord
      const point = latLngToVector3(lat, lng, 1)
      
      // پروجکشن ساده روی صفحه مماسی
      const u = (lng + 180) / 360
      const v = (lat + 90) / 180
      
      if (i === 0) {
        shape.moveTo(u * 2 - 1, v * 2 - 1)
      } else {
        shape.lineTo(u * 2 - 1, v * 2 - 1)
      }
    })

    const geometry = new THREE.ShapeGeometry(shape)
    
    // رنگ کشور
    const defaultColor = continentColors[continent] || '#E0E0E0'
    const color = props.colorMap[iso] || defaultColor
    
    const material = new THREE.MeshPhongMaterial({
      color: new THREE.Color(color),
      flatShading: true,
      side: THREE.DoubleSide
    })
    
    const countryMesh = new THREE.Mesh(geometry, material)
    
    // تبدیل به مش کروی
    const positions = geometry.attributes.position.array
    for (let i = 0; i < positions.length; i += 3) {
      const u = (positions[i] + 1) / 2
      const v = (positions[i + 1] + 1) / 2
      
      const lng = u * 360 - 180
      const lat = v * 180 - 90
      
      const vec = latLngToVector3(lat, lng, 1.01)
      positions[i] = vec.x
      positions[i + 1] = vec.y
      positions[i + 2] = vec.z
    }
    geometry.attributes.position.needsUpdate = true
    geometry.computeVertexNormals()
    
    // خطوط مرزی
    const edges = new THREE.EdgesGeometry(geometry)
    const lineMaterial = new THREE.LineBasicMaterial({ 
      color: 0x000000, 
      linewidth: 2 
    })
    const line = new THREE.LineSegments(edges, lineMaterial)
    countryMesh.add(line)
    
    countryMesh.userData = { name, iso, continent, originalColor: color }
    countries.push(countryMesh)
    globe.add(countryMesh)
  })
}

function updateCountryColors() {
  countries.forEach(country => {
    const iso = country.userData.iso
    if (props.colorMap[iso]) {
      const newColor = new THREE.Color(props.colorMap[iso])
      country.material.color = newColor
      country.userData.originalColor = props.colorMap[iso]
    }
  })
}

function onCountryClick(event) {
  const rect = canvasRef.value.getBoundingClientRect()
  mouse.x = ((event.clientX || event.changedTouches[0].clientX) - rect.left) / rect.width * 2 - 1
  mouse.y = -((event.clientY || event.changedTouches[0].clientY) - rect.top) / rect.height * 2 + 1

  raycaster.setFromCamera(mouse, camera)
  const intersects = raycaster.intersectObjects(countries)

  // حذف highlight قبلی
  countries.forEach(c => {
    c.material.emissive = new THREE.Color(0x000000)
  })

  if (intersects.length > 0) {
    const country = intersects[0].object
    country.material.emissive = new THREE.Color(0xFFAA00)
    
    selectedCountry.value = {
      name: country.userData.name,
      iso: country.userData.iso
    }
    
    emit('selectedCountry', selectedCountry.value)
    
    setTimeout(() => {
      selectedCountry.value = null
    }, 3000)
  }
}

function animate() {
  animationId = requestAnimationFrame(animate)
  
  // چرخش خودکار
  if (props.autoRotate && !isUserInteracting && Date.now() - lastInteractionTime > 2000) {
    globe.rotation.y += props.rotationSpeed
  }
  
  controls.update()
  renderer.render(scene, camera)
}

function handleResize() {
  if (!containerRef.value) return
  
  const width = containerRef.value.clientWidth
  const height = containerRef.value.clientHeight
  
  camera.aspect = width / height
  camera.updateProjectionMatrix()
  renderer.setSize(width, height)
}
</script>

<style scoped>
.earth-container {
  width: 100%;
  height: 100vh;
  position: relative;
  overflow: hidden;
  background: #FFFFFF;
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
}

.country-info {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(255, 255, 255, 0.95);
  padding: 15px 25px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  border: 2px solid #000;
  animation: slideIn 0.3s ease-out;
}

.country-info h3 {
  margin: 0 0 8px 0;
  font-size: 18px;
  font-weight: 700;
  color: #000;
}

.country-info p {
  margin: 0;
  font-size: 14px;
  color: #666;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>