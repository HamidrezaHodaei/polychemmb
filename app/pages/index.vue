<template>
  <div>
    <Navbar/>
    <Hero/>
    <div class="mt-8 slider">
      <Slider/>
    </div>
  </div>
</template>

<script setup>
  import Navbar from "~/components/navbar.vue"
  import Slider from "~/components/Slider.vue"
  import Hero from "~/components/Hero.vue";
</script>