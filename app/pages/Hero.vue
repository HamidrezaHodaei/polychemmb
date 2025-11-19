<template>
  <main class="min-h-screen flex items-center justify-center bg-[#f1f2f2]">
    <div class="flex flex-col sm:flex-row items-center justify-center text-center sm:text-left px-4">
      <p class="text-2xl sm:text-4xl md:text-5xl font-bold text-[#848484] uppercase mb-4 sm:mb-0">
        Polychem —
      </p>
      <section class="h-12 sm:h-16 md:h-20 overflow-hidden sm:ml-4">
        <div class="animate-text-slide">
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Polymer Art.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Originality.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Loyalty.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Yearn.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Creativity.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Honesty.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Elegance.
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Where every grain of polymer becomes Meaningful.
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup>
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@700&display=swap');

* {
  font-family: 'Roboto', sans-serif;
}

.animate-text-slide {
  animation: text-animation 32s infinite;
}

@keyframes text-animation {
  0% { margin-top: 0; }
  10% { margin-top: 0; }
  12.5% { margin-top: -3rem; }
  22.5% { margin-top: -3rem; }
  25% { margin-top: -6rem; }
  35% { margin-top: -6rem; }
  37.5% { margin-top: -9rem; }
  47.5% { margin-top: -9rem; }
  50% { margin-top: -12rem; }
  60% { margin-top: -12rem; }
  62.5% { margin-top: -15rem; }
  72.5% { margin-top: -15rem; }
  75% { margin-top: -18rem; }
  85% { margin-top: -18rem; }
  87.5% { margin-top: -21rem; }
  97.5% { margin-top: -21rem; }
  100% { margin-top: 0; }
}

@media (min-width: 640px) {
  @keyframes text-animation {
    0% { margin-top: 0; }
    10% { margin-top: 0; }
    12.5% { margin-top: -4rem; }
    22.5% { margin-top: -4rem; }
    25% { margin-top: -8rem; }
    35% { margin-top: -8rem; }
    37.5% { margin-top: -12rem; }
    47.5% { margin-top: -12rem; }
    50% { margin-top: -16rem; }
    60% { margin-top: -16rem; }
    62.5% { margin-top: -20rem; }
    72.5% { margin-top: -20rem; }
    75% { margin-top: -24rem; }
    85% { margin-top: -24rem; }
    87.5% { margin-top: -28rem; }
    97.5% { margin-top: -28rem; }
    100% { margin-top: 0; }
  }
}

@media (min-width: 768px) {
  @keyframes text-animation {
    0% { margin-top: 0; }
    10% { margin-top: 0; }
    12.5% { margin-top: -5rem; }
    22.5% { margin-top: -5rem; }
    25% { margin-top: -10rem; }
    35% { margin-top: -10rem; }
    37.5% { margin-top: -15rem; }
    47.5% { margin-top: -15rem; }
    50% { margin-top: -20rem; }
    60% { margin-top: -20rem; }
    62.5% { margin-top: -25rem; }
    72.5% { margin-top: -25rem; }
    75% { margin-top: -30rem; }
    85% { margin-top: -30rem; }
    87.5% { margin-top: -35rem; }
    97.5% { margin-top: -35rem; }
    100% { margin-top: 0; }
  }
}
</style>