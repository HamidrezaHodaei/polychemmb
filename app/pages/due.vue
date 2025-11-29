<template>
  <div style="width: 100%; height: 100vh; position: relative;">
    <div ref="mapContainer" style="width: 100%; height: 100%;"></div>
    
    <!-- دکمه مسیریابی -->
    <div class="routing-button">
      <button @click="getDirections" class="btn-directions">
        🧭 مسیریابی به این نقطه
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const mapContainer = ref(null);
let map = null;
let routingControl = null;

// مختصات مقصد
const destLat = 38.92111436229225;
const destLng = 45.6492204411566;

onMounted(() => {
  // لود کردن CSS های Leaflet
  const link1 = document.createElement('link');
  link1.rel = 'stylesheet';
  link1.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
  document.head.appendChild(link1);

  // لود کردن CSS مسیریابی
  const link2 = document.createElement('link');
  link2.rel = 'stylesheet';
  link2.href = 'https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.css';
  document.head.appendChild(link2);

  // لود کردن JS Leaflet
  const script1 = document.createElement('script');
  script1.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
  script1.onload = () => {
    // لود کردن JS مسیریابی
    const script2 = document.createElement('script');
    script2.src = 'https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.js';
    script2.onload = initMap;
    document.head.appendChild(script2);
  };
  document.head.appendChild(script1);
});

const initMap = () => {
  // ایجاد نقشه
  map = L.map(mapContainer.value, {
    center: [destLat, destLng],
    zoom: 9,
    zoomControl: true,
    attributionControl: false
  });

  // اضافه کردن لایه روشن
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '© OpenStreetMap'
  }).addTo(map);

  // آیکون لوکیشن زرد ساده بدون انیمیشن
  const pointerIcon = L.divIcon({
    html: `
      <svg width="40" height="50" viewBox="0 0 40 50" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <filter id="shadow">
            <feGaussianBlur in="SourceAlpha" stdDeviation="2"/>
            <feOffset dx="0" dy="2" result="offsetblur"/>
            <feComponentTransfer>
              <feFuncA type="linear" slope="0.4"/>
            </feComponentTransfer>
            <feMerge>
              <feMergeNode/>
              <feMergeNode in="SourceGraphic"/>
            </feMerge>
          </filter>
        </defs>
        
        <!-- شکل پین -->
        <path d="M20 5 C12 5, 7 10, 7 17 C7 26, 20 45, 20 45 C20 45, 33 26, 33 17 C33 10, 28 5, 20 5 Z" 
              fill="#FFD700" 
              stroke="#FFA500" 
              stroke-width="2.5" 
              filter="url(#shadow)"/>
        
        <!-- دایره داخلی -->
        <circle cx="20" cy="17" r="5" fill="#FFA500"/>
        <circle cx="20" cy="17" r="3" fill="#FFF" opacity="0.7"/>
      </svg>
    `,
    className: 'pointer-wrapper',
    iconSize: [40, 50],
    iconAnchor: [20, 45]
  });

  // اضافه کردن مارکر مقصد
  L.marker([destLat, destLng], { icon: pointerIcon }).addTo(map);
};

const getDirections = () => {
  if (!map) return;

  // پاک کردن مسیر قبلی
  if (routingControl) {
    map.removeControl(routingControl);
  }

  // دریافت موقعیت فعلی کاربر
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        const userLat = position.coords.latitude;
        const userLng = position.coords.longitude;

        // ایجاد مسیریابی
        routingControl = L.Routing.control({
          waypoints: [
            L.latLng(userLat, userLng),
            L.latLng(destLat, destLng)
          ],
          routeWhileDragging: true,
          showAlternatives: true,
          language: 'fa',
          lineOptions: {
            styles: [{
              color: '#FFD700',
              opacity: 0.8,
              weight: 6
            }]
          },
          createMarker: function(i, waypoint, n) {
            const marker = L.marker(waypoint.latLng, {
              draggable: false,
              icon: L.icon({
                iconUrl: i === 0 ? 
                  'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-green.png' :
                  'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-gold.png',
                shadowUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png',
                iconSize: [25, 41],
                iconAnchor: [12, 41],
                popupAnchor: [1, -34],
                shadowSize: [41, 41]
              })
            });
            
            if (i === 0) {
              marker.bindPopup('📍 موقعیت شما');
            } else {
              marker.bindPopup('🎯 مقصد');
            }
            
            return marker;
          }
        }).addTo(map);

        // زوم به مسیر
        map.fitBounds([
          [userLat, userLng],
          [destLat, destLng]
        ], { padding: [50, 50] });
      },
      (error) => {
        alert('⚠️ دسترسی به موقعیت مکانی شما امکان‌پذیر نیست. لطفاً دسترسی GPS را فعال کنید.');
        console.error('Geolocation error:', error);
      },
      {
        enableHighAccuracy: true,
        timeout: 10000,
        maximumAge: 0
      }
    );
  } else {
    alert('⚠️ مرورگر شما از GPS پشتیبانی نمی‌کند.');
  }
};
</script>

<style scoped>
.pointer-wrapper {
  background: transparent !important;
  border: none !important;
}

.pointer-wrapper svg {
  filter: drop-shadow(0 4px 8px rgba(255, 165, 0, 0.4));
}

.routing-button {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 1000;
}

.btn-directions {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 100%);
  color: #333;
  border: none;
  padding: 14px 24px;
  border-radius: 12px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(255, 165, 0, 0.4);
  transition: all 0.3s ease;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.btn-directions:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 165, 0, 0.6);
  background: linear-gradient(135deg, #FFA500 0%, #FFD700 100%);
}

.btn-directions:active {
  transform: translateY(0);
  box-shadow: 0 2px 10px rgba(255, 165, 0, 0.4);
}

.leaflet-control-zoom a {
  background: rgba(255, 255, 255, 0.95) !important;
  color: #333 !important;
  border: 1px solid rgba(0, 0, 0, 0.2) !important;
  width: 34px !important;
  height: 34px !important;
  line-height: 34px !important;
  font-size: 20px !important;
  font-weight: bold !important;
}

.leaflet-control-zoom a:hover {
  background: rgba(255, 215, 0, 0.2) !important;
  border-color: #FFA500 !important;
}

.leaflet-container {
  background: #f5f5f5 !important;
}

/* استایل پنل مسیریابی */
:deep(.leaflet-routing-container) {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  padding: 10px;
  direction: rtl;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

:deep(.leaflet-routing-alt) {
  background: #fff;
  border-radius: 8px;
  margin: 5px 0;
  padding: 10px;
  border: 2px solid #FFD700;
}

:deep(.leaflet-routing-alt:hover) {
  background: #fffef0;
}

/* رنگ خط مسیر */
:deep(.leaflet-routing-line) {
  stroke: #FFD700 !important;
  stroke-width: 6 !important;
}

@media (max-width: 768px) {
  .routing-button {
    top: 10px;
    right: 10px;
    left: 10px;
  }
  
  .btn-directions {
    width: 100%;
    padding: 12px 20px;
    font-size: 14px;
  }
}
</style>