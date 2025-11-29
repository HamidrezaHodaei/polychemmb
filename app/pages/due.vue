<template>
  <div ref="mapContainer" style="width: 100%; height: 100vh;"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const mapContainer = ref(null);

onMounted(() => {
  // لود کردن CSS
  const link = document.createElement('link');
  link.rel = 'stylesheet';
  link.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
  document.head.appendChild(link);

  // لود کردن JS
  const script = document.createElement('script');
  script.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
  script.onload = () => {
    // مختصات
    const lat = 38.92111436229225;
    const lng = 45.6492204411566;

    // ایجاد نقشه
    const map = L.map(mapContainer.value, {
      center: [lat, lng],
      zoom: 13,
      zoomControl: true,
      attributionControl: false
    });

    // اضافه کردن لایه Dark Mode
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      maxZoom: 19,
      subdomains: 'abcd'
    }).addTo(map);

    // آیکون پوینتر سفارشی
    const pointerIcon = L.divIcon({
      html: '<div class="custom-pointer"></div>',
      className: 'pointer-wrapper',
      iconSize: [40, 40],
      iconAnchor: [20, 20]
    });

    // اضافه کردن مارکر
    L.marker([lat, lng], { icon: pointerIcon }).addTo(map);
  };
  document.head.appendChild(script);
});
</script>

<style>
.pointer-wrapper {
  background: transparent !important;
  border: none !important;
}

.custom-pointer {
  position: relative;
  width: 20px;
  height: 20px;
  background: radial-gradient(circle, #00d9ff 0%, #0066ff 100%);
  border: 3px solid #ffffff;
  border-radius: 50%;
  box-shadow: 
    0 0 0 8px rgba(0, 217, 255, 0.2),
    0 4px 15px rgba(0, 0, 0, 0.4),
    0 0 30px rgba(0, 217, 255, 0.5);
  animation: pulse 2s ease-in-out infinite;
}

.custom-pointer::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 40px;
  height: 40px;
  border: 2px solid rgba(0, 217, 255, 0.6);
  border-radius: 50%;
  animation: wave 2s ease-out infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    box-shadow: 
      0 0 0 8px rgba(0, 217, 255, 0.2),
      0 4px 15px rgba(0, 0, 0, 0.4),
      0 0 30px rgba(0, 217, 255, 0.5);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 
      0 0 0 10px rgba(0, 217, 255, 0.3),
      0 6px 20px rgba(0, 0, 0, 0.5),
      0 0 40px rgba(0, 217, 255, 0.7);
  }
}

@keyframes wave {
  0% {
    transform: translate(-50%, -50%) scale(0.5);
    opacity: 1;
  }
  100% {
    transform: translate(-50%, -50%) scale(2);
    opacity: 0;
  }
}

.leaflet-control-zoom a {
  background: rgba(20, 20, 30, 0.9) !important;
  color: #00d9ff !important;
  border: 1px solid rgba(0, 217, 255, 0.3) !important;
  width: 34px !important;
  height: 34px !important;
  line-height: 34px !important;
  font-size: 20px !important;
  font-weight: bold !important;
}

.leaflet-control-zoom a:hover {
  background: rgba(0, 217, 255, 0.2) !important;
  border-color: #00d9ff !important;
}

.leaflet-container {
  background: #0a0a0a !important;
}
</style>