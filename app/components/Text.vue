<template>
  <main class="min-h-screen flex items-center justify-center bg-[#f1f2f2] force-font-size">
    <div class="flex flex-col sm:flex-row items-center justify-center text-center sm:text-left px-4">
      <p class="text-2xl sm:text-4xl md:text-5xl font-bold text-[#848484] uppercase mb-4 sm:mb-0">
        <span class="brand">Polychem</span><span class="brand-separator"> — </span><span class="brand-rest">Where every grain of polymer becomes</span>
      </p>
      <section class="h-12 sm:h-16 md:h-20 overflow-hidden sm:ml-4">
        <div class="animate-text-slide">
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Polymer Art
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Originality
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 w-40 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Loyalty
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 w-45 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Creativity
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Honesty
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Elegance
          </div>
          <div class="inline-block px-3 py-1 sm:px-4 sm:py-2 h-12 sm:h-16 md:h-20 mb-12 sm:mb-16 md:mb-20 bg-[#FFD700] text-white text-lg sm:text-2xl md:text-3xl font-bold uppercase flex items-center">
            Meaningful
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup>
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700;800&display=swap');

* {
  font-family: 'Montserrat', sans-serif;
}

.force-font-size {
  --base-font-size: 2.2rem;
  font-size: var(--base-font-size) !important;
}

.force-font-size *:not(.brand):not(.brand *) {
  font-size: inherit !important;
}

.brand {
  font-weight: 800;
  font-size: calc(var(--base-font-size) * 1.35) !important;
  letter-spacing: 0.02em;
}

.brand-separator {
  font-weight: 700;
  margin: 0 0.25rem;
}

.brand-rest {
  font-weight: 700;
  font-size: calc(var(--base-font-size) * 0.95);
}

@media (min-width: 640px) {
  .brand {
    font-size: calc(var(--base-font-size) * 1.45) !important;
  }
  .brand-rest {
    font-size: calc(var(--base-font-size) * 1);
  }
}

@media (min-width: 768px) {
  .brand {
    font-size: calc(var(--base-font-size) * 1.6) !important;
  }
  .brand-rest {
    font-size: calc(var(--base-font-size) * 1.05);
  }
}

.animate-text-slide {
  animation: text-animation 20s infinite;
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