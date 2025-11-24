<template>
    <div class="slider">
      <div class="slide-track">
        <div v-for="(img, index) in images" :key="index" class="slide">
          <img :src="img" height="100" width="250" alt="logo" />
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const images = [
      "/logoMegabiz.jpg",
  ];
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
      transform: translateX(calc(-250px * 7));
    }
  }
  
  .slider {
    background: white;
    box-shadow: 0 10px 20px -5px rgba(0, 0, 0, 0.125);
    height: 100px;
    margin: auto;
    overflow: hidden;
    position: relative;
    width: 1700px;
  
    &::before,
    &::after {
      @include white-gradient;
      content: "";
      height: 100px;
      position: absolute;
      width: 200px;
      z-index: 2;
    }
  
    &::after {
      right: 0;
      top: 0;
      transform: rotateZ(180deg);
    }
  
    &::before {
      left: 0;
      top: 0;
    }
  
    .slide-track {
      animation: scroll $animationSpeed linear infinite;
      display: flex;
      width: calc(250px * 14);
    }
  
    .slide {
      height: 100px;
      width: 250px;
    }
    
    .slide img {
      filter: grayscale(100%);
      transition: filter 0.3s ease;
    }
  
    .slide:hover img {
      filter: grayscale(0%);
    }
  }
  </style>
