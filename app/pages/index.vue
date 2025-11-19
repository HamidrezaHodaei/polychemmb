<template>
  <div>
    <Navbar/>
    <Hero/>
    <div class="mt-8 slider">
      <Slider/>
      <Text/>
      <Cards/>
      <infinityslider/>
    </div>
  </div>
</template>

<script setup>
  import Navbar from "~/components/navbar.vue"
  import Slider from "~/components/Slider.vue"
  import Hero from "~/components/Hero.vue"
  import Cards from "~/components/cards.vue"
  import Text from "~/components/Text.vue"
  import infinityslider from "~/components/InfiniteSlider.vue";
</script>