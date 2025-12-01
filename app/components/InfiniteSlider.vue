<template>
    <div class="slider">
      <div class="slide-track">
        <div v-for="(img, index) in images" :key="index" class="slide">
          <!-- changed: add class and remove fixed height/width attributes -->
          <img :src="img" alt="logo" class="logo" />
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const baseImages = [
      "/Olsanbaft.png",
      "/Reyhaneh.png",
      "/Paraplastic.png"
  ];
  // تکرار آرایه برای اسکرول بی‌نهایت بدون فاصله سفید
  const images = [...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages, ...baseImages];
  </script>
  
  <style scoped lang="scss">
  body {
    align-items: center;
    background: #e3e3e3;
    display: flex;
    height: 100vh;
    justify-content: center;
  }
  
  @mixin white-gradient {
    background: linear-gradient(to right, rgba(255, 255, 255, 1) 0%, rgba(255, 255, 255, 0) 100%);
  }
  
  $animationSpeed: 40s;
  
  @keyframes scroll {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(calc(-250px * 10));
    }
  }
  
  .slider {
    background: white;
    box-shadow: 0 10px 20px -5px rgba(0, 0, 0, 0.125);
    height: 100px;
    margin: auto;
    overflow: hidden;
    position: relative;
    width: 100%;
  
    .slide-track {
      animation: scroll $animationSpeed linear infinite;
      display: flex;
      width: calc(250px * 20);
    }
  
    .slide {
      height: 100px;
      width: 250px;
      flex-shrink: 0;
      /* changed: center content so logos align vertically */
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 8px; /* slight horizontal padding to avoid touching edges */
    }
    
    /* changed: target .logo class and constrain size while preserving aspect ratio */
    .logo {
      max-height: 72px;
      max-width: 220px;
      width: auto;
      height: auto;
      object-fit: contain;
      display: block;
      filter: grayscale(100%);
      transition: filter 0.3s ease;
    }
  
    .slide:hover .logo {
      filter: grayscale(0%);
    }
  }
  </style>
