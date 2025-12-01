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
   "/27_full-0۱.jpg",    
  "/Olsanbaft.png",
  "/Paraplastic.png",    
  "/Reyhaneh.png",
  "/logoMegabiz.jpg"
      
      
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
    margin: 0; // حذف فاصله اطراف
    padding: 0; // حذف padding احتمالی
    overflow: hidden;
    position: relative;
    width: 100vw; // پر کردن کل عرض صفحه

    .slide-track {
      animation: scroll $animationSpeed linear infinite;
      display: flex;
      width: calc(250px * 20);
    }
  
    .slide {
      height: 100px;
      width: auto;
      flex-shrink: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 21px; // فاصله افقی خیلی کم
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
