<template>
  <div style="width: 100%; height: 450px; position: relative; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
    <div ref="mapContainer" style="width: 100%; height: 100%;"></div>
    
    <!-- Navigation Button -->
    <div class="routing-button">
      <button @click="getDirections" class="btn-directions">
        <span>Get Directions</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const mapContainer = ref(null);
let map = null;
let routingControl = null;

const destLat = 38.92111436229225;
const destLng = 45.6492204411566;

onMounted(() => {
  const link1 = document.createElement('link');
  link1.rel = 'stylesheet';
  link1.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
  document.head.appendChild(link1);

  const link2 = document.createElement('link');
  link2.rel = 'stylesheet';
  link2.href = 'https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.css';
  document.head.appendChild(link2);

  const script1 = document.createElement('script');
  script1.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
  script1.onload = () => {
    const script2 = document.createElement('script');
    script2.src = 'https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.js';
    script2.onload = initMap;
    document.head.appendChild(script2);
  };
  document.head.appendChild(script1);
});

const initMap = () => {
  map = L.map(mapContainer.value, {
    center: [destLat, destLng],
    zoom: 13,
    zoomControl: true,
    attributionControl: false
  });

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '© OpenStreetMap'
  }).addTo(map);

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
        
        <path d="M20 5 C12 5, 7 10, 7 17 C7 26, 20 45, 20 45 C20 45, 33 26, 33 17 C33 10, 28 5, 20 5 Z" 
              fill="#FFD700" 
              stroke="#808080" 
              stroke-width="2.5" 
              filter="url(#shadow)"/>
        
        <circle cx="20" cy="17" r="5" fill="#FFA500"/>
        <circle cx="20" cy="17" r="3" fill="#FFF" opacity="0.7"/>
      </svg>
    `,
    className: 'pointer-wrapper',
    iconSize: [40, 50],
    iconAnchor: [20, 45]
  });

  L.marker([destLat, destLng], { icon: pointerIcon }).addTo(map);
};

const getDirections = () => {
  if (!map) return;

  if (routingControl) {
    map.removeControl(routingControl);
  }

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        const userLat = position.coords.latitude;
        const userLng = position.coords.longitude;

        routingControl = L.Routing.control({
          waypoints: [
            L.latLng(userLat, userLng),
            L.latLng(destLat, destLng)
          ],
          routeWhileDragging: true,
          showAlternatives: true,
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
              marker.bindPopup('📍 Your Location');
            } else {
              marker.bindPopup('🎯 Destination');
            }
            
            return marker;
          }
        }).addTo(map);

        map.fitBounds([
          [userLat, userLng],
          [destLat, destLng]
        ], { padding: [50, 50] });
      },
      (error) => {
        alert('⚠️ Location access denied. Please enable GPS.');
        console.error('Geolocation error:', error);
      },
      {
        enableHighAccuracy: true,
        timeout: 10000,
        maximumAge: 0
      }
    );
  } else {
    alert('⚠️ Your browser does not support GPS.');
  }
};
</script>

<style scoped>
/* Map container should stay below navbar (z-50) */
div[style*="position: relative"] {
  z-index: 1;
}

.pointer-wrapper {
  background: transparent !important;
  border: none !important;
}

.pointer-wrapper svg {
  filter: drop-shadow(0 4px 8px rgba(128, 128, 128, 0.3));
}

.routing-button {
  position: absolute;
  bottom: 15px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
}

.btn-directions {
  position: relative;
  overflow: hidden;
  background: #f3f4f6;
  color: #848484;
  border: 1px solid #d1d5db;
  padding: 10px 20px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: color 300ms, border-color 300ms, background-color 300ms;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  white-space: nowrap;
  z-index: 1;
}

.btn-directions::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #FFCD05;
  transform: translateY(-100%);
  transition: transform 300ms ease;
  z-index: 0;
}

.btn-directions:hover::before,
.btn-directions:focus-visible::before {
  transform: translateY(0);
}

.btn-directions:hover,
.btn-directions:focus-visible {
  color: #fff;
  border-color: #FFCD05;
  outline: none;
}

.btn-directions span {
  position: relative;
  z-index: 1;
}

.leaflet-control-zoom a {
  background: rgba(255, 255, 255, 0.95) !important;
  color: #333 !important;
  border: 1px solid rgba(0, 0, 0, 0.2) !important;
  width: 30px !important;
  height: 30px !important;
  line-height: 30px !important;
  font-size: 18px !important;
}

.leaflet-control-zoom a:hover {
  background: rgba(255, 215, 0, 0.2) !important;
  border-color: #FFA500 !important;
}

.leaflet-container {
  background: #f5f5f5 !important;
}

:deep(.leaflet-routing-container) {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
  padding: 8px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 13px;
}

:deep(.leaflet-routing-alt) {
  background: #fff;
  border-radius: 6px;
  margin: 4px 0;
  padding: 8px;
  border: 2px solid #FFD700;
}

:deep(.leaflet-routing-alt:hover) {
  background: #fffef0;
}

:deep(.leaflet-routing-line) {
  stroke: #FFD700 !important;
  stroke-width: 6 !important;
}
</style>