<template>
  <div class="map-container">
    <div ref="mapElement" class="map"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const mapElement = ref(null);
let map = null;
let marker = null;

// مختصات دقیق
const lat = 38.92111436229225;
const lng = 45.6492204411566;

onMounted(async () => {
  // بارگذاری Leaflet از CDN
  if (!window.L) {
    // بارگذاری CSS
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css';
    document.head.appendChild(link);

    // بارگذاری JS
    const script = document.createElement('script');
    script.src = 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js';
    script.onload = initMap;
    document.body.appendChild(script);
  } else {
    initMap();
  }
});

const initMap = () => {
  if (!mapElement.value) return;

  // ایجاد نقشه
  map = window.L.map(mapElement.value, {
    center: [lat, lng],
    zoom: 13,
    zoomControl: true
  });

  // اضافه کردن لایه Dark Mode
  // استفاده از Carto Dark Matter
  window.L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
    subdomains: 'abcd',
    maxZoom: 20
  }).addTo(map);

  // ایجاد آیکون سفارشی برای مارکر
  const customIcon = window.L.divIcon({
    className: 'custom-marker',
    html: `
      <div style="
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        width: 40px;
        height: 40px;
        border-radius: 50% 50% 50% 0;
        transform: rotate(-45deg);
        border: 3px solid #fff;
        box-shadow: 0 4px 12px rgba(0,0,0,0.4);
        display: flex;
        align-items: center;
        justify-content: center;
      ">
        <div style="
          transform: rotate(45deg);
          color: white;
          font-size: 20px;
          margin-top: -5px;
        ">📍</div>
      </div>
    `,
    iconSize: [40, 40],
    iconAnchor: [20, 40],
    popupAnchor: [0, -40]
  });

  // اضافه کردن مارکر
  marker = window.L.marker([lat, lng], { icon: customIcon }).addTo(map);

  // محتوای پاپ‌آپ به فارسی
  const popupContent = `
    <div style="
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      direction: rtl;
      text-align: right;
      padding: 8px;
      min-width: 200px;
    ">
      <h3 style="
        margin: 0 0 10px 0;
        color: #667eea;
        font-size: 16px;
        border-bottom: 2px solid #667eea;
        padding-bottom: 5px;
      ">📍 موقعیت مکانی</h3>
      <p style="margin: 8px 0; font-size: 13px;">
        <strong>عرض جغرافیایی:</strong> ${lat}
      </p>
      <p style="margin: 8px 0; font-size: 13px;">
        <strong>طول جغرافیایی:</strong> ${lng}
      </p>
      <p style="
        margin: 12px 0 5px 0;
        color: #666;
        font-size: 12px;
        font-style: italic;
      ">
        این موقعیت در شمال‌غرب ایران قرار دارد 🗺️
      </p>
    </div>
  `;

  marker.bindPopup(popupContent, {
    maxWidth: 300,
    className: 'custom-popup'
  });

  // نمایش خودکار پاپ‌آپ
  marker.openPopup();

  // افکت انیمیشن برای مارکر
  setTimeout(() => {
    marker.setLatLng([lat, lng]);
  }, 100);
};

onUnmounted(() => {
  if (map) {
    map.remove();
    map = null;
  }
});
</script>

<style scoped>
.map-container {
  width: 100%;
  height: 100vh;
  position: relative;
  background: #1a1a2e;
}

.map {
  width: 100%;
  height: 100%;
  border-radius: 0;
}

:deep(.leaflet-popup-content-wrapper) {
  background: #2d2d44;
  color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.6);
}

:deep(.leaflet-popup-tip) {
  background: #2d2d44;
}

:deep(.leaflet-control-zoom a) {
  background: #2d2d44 !important;
  color: #fff !important;
  border: 1px solid #444 !important;
}

:deep(.leaflet-control-zoom a:hover) {
  background: #3d3d54 !important;
}

:deep(.custom-marker) {
  animation: bounce 2s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

:deep(.leaflet-control-attribution) {
  background: rgba(45, 45, 68, 0.8) !important;
  color: #aaa !important;
}

:deep(.leaflet-control-attribution a) {
  color: #667eea !important;
}
</style>