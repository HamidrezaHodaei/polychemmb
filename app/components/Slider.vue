<template>
  <div class="container mx-auto px-4 font-sans text-red-100]">
    <div class="w-full">
      <div
        ref="cardsContainer"
        class="relative flex justify-center items-center h-[80vh] mb-24"
      >
        <!-- Card 1 -->
        <div
          ref="card1"
          class="absolute w-[100%] md:w-[95%] h-[85vh] rounded-[50px] shadow-2xl flex flex-col md:flex-row items-stretch overflow-hidden"
          style="top: 0px; z-index: 2;"
        >
          <!-- Left: Image (60% on desktop, 100% on mobile) -->
          <div class="w-full md:w-3/5 overflow-hidden rounded-t-[50px] md:rounded-tl-[50px] md:rounded-bl-[50px] md:rounded-tr-[0px] md:rounded-br-[0px]">
            <img src="/white-plastic-polymer.jpg" alt="Product 1" class="w-full h-full object-cover" />
          </div>

          <!-- Right: Content (40% on desktop) -->
          <div class="w-full md:w-2/5 p-8 flex flex-col justify-center gap-4 card-content">
            <h2 class="text-3xl md:text-4xl font-semibold text-[#848484]}">Product One</h2>
            <p class="text-base md:text-lg text-[#848484]">A short description that highlights the main feature of this product in one or two lines.</p>
            <div class="text-2xl font-semibold text-[#848484]">€19.99</div>
            <button class="btn-slide-down w-full md:w-auto h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors max-w-[220px]">
              <span class="relative z-10 font-medium">Buy Now</span>
            </button>
          </div>
        </div>

        <!-- Card 2 -->
        <div
          ref="card2"
          class="absolute w-[100%] md:w-[95%] h-[85vh] rounded-[50px] shadow-2xl flex flex-col md:flex-row items-stretch overflow-hidden"
          style="top: 30px; z-index: 3;"
        >
          <div class="w-full md:w-3/5 overflow-hidden rounded-t-[50px] md:rounded-tl-[50px] md:rounded-bl-[50px] md:rounded-tr-[0px] md:rounded-br-[0px]">
            <img src="/white-plastic-polymer.jpg" alt="Product 2" class="w-full h-full object-cover" />
          </div>

          <div class="w-full md:w-2/5 p-8 flex flex-col justify-center gap-4 card-content">
            <h2 class="text-3xl md:text-4xl font-semibold text-[#848484]">Product Two</h2>
            <p class="text-base md:text-lg text-[#848484]">Concise description that communicates value and key use-cases.</p>
            <div class="text-2xl font-semibold text-[#848484]">€29.99</div>
            <button class="btn-slide-down w-full md:w-auto h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors max-w-[220px]">
              <span class="relative z-10 font-medium">Buy Now</span>
            </button>
          </div>
        </div>

        <!-- Card 3 -->
        <div
          ref="card3"
          class="absolute w-[100%] md:w-[95%] h-[85vh] rounded-[50px] shadow-2xl flex flex-col md:flex-row items-stretch overflow-hidden"
          style="top: 60px; z-index: 4;"
        >
          <div class="w-full md:w-3/5 overflow-hidden rounded-t-[50px] md:rounded-tl-[50px] md:rounded-bl-[50px] md:rounded-tr-[0px] md:rounded-br-[0px]">
            <img src="/white-plastic-polymer.jpg" alt="Product 3" class="w-full h-full object-cover" />
          </div>

          <div class="w-full md:w-2/5 p-8 flex flex-col justify-center gap-4 card-content">
            <h2 class="text-3xl md:text-4xl font-semibold text-[#848484]">Product Three</h2>
            <p class="text-base md:text-lg text-[#848484]">Short, punchy description emphasizing benefits and uniqueness.</p>
            <div class="text-2xl font-semibold text-[#848484]">€39.99</div>
            <button class="btn-slide-down w-full md:w-auto h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors max-w-[220px]">
              <span class="relative z-10 font-medium">Buy Now</span>
            </button>
          </div>
        </div>

        <!-- Card 4 -->
        <div
          ref="card4"
          class="absolute w-[100%] md:w-[95%] h-[85vh] rounded-[50px] shadow-2xl flex flex-col md:flex-row items-stretch overflow-hidden"
          style="top: 90px; z-index: 5;"
        >
          <div class="w-full md:w-3/5 overflow-hidden rounded-t-[50px] md:rounded-tl-[50px] md:rounded-bl-[50px] md:rounded-tr-[0px] md:rounded-br-[0px]">
            <img src="/white-plastic-polymer.jpg" alt="Product 4" class="w-full h-full object-cover" />
          </div>

          <div class="w-full md:w-2/5 p-8 flex flex-col justify-center gap-4 card-content">
            <h2 class="text-3xl md:text-4xl font-semibold text-[#848484]">Product Four</h2>
            <p class="text-base md:text-lg text-[#848484]">One-liner that explains why this product stands out.</p>
            <div class="text-2xl font-semibold text-[#848484]">€49.99</div>
            <button class="btn-slide-down w-full md:w-auto h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors max-w-[220px]">
              <span class="relative z-10 font-medium">Buy Now</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { ScrollToPlugin } from 'gsap/ScrollToPlugin'

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin)

const cardsContainer = ref(null)
const card1 = ref(null)
const card2 = ref(null)
const card3 = ref(null)
const card4 = ref(null)

let tl = null

onMounted(() => {
  tl = gsap.timeline({
    scrollTrigger: {
      trigger: cardsContainer.value,
      pin: true,
      pinSpacing: true,
      markers: false,
      start: 'top-=50px top',
      end: '+=3500',
      scrub: 1,
    },
  })

  tl.addLabel('card1')
  tl.to(card1.value, {
    yPercent: 0,
    opacity: 1,
    duration: 1,
    ease: 'power2.out',
  })

  tl.from(
    card2.value,
    {
      yPercent: 75,
      opacity: 0,
      duration: 1.5,
      ease: 'power2.out',
    },
    '+=0.2'
  )

  tl.addLabel('card2')

  tl.to(
    card1.value,
    {
      scale: 0.925,
      yPercent: -0.75,
      opacity: 0.85,
      duration: 1.2,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )

  tl.to(
    card2.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 1.2,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  tl.from(
    card3.value,
    {
      yPercent: 75,
      opacity: 0,
      duration: 1.5,
      ease: 'power2.out',
    },
    '+=0.2'
  )

  tl.addLabel('card3')

  tl.to(
    card2.value,
    {
      scale: 0.95,
      yPercent: -0.5,
      opacity: 0.85,
      duration: 1.2,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )

  tl.to(
    card3.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 1.2,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  tl.from(
    card4.value,
    {
      yPercent: 75,
      opacity: 0,
      duration: 1.5,
      ease: 'power2.out',
    },
    '+=0.2'
  )

  tl.addLabel('card4')

  tl.to(
    card3.value,
    {
      scale: 0.975,
      yPercent: -0.4,
      opacity: 0.85,
      duration: 1.2,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )

  tl.to(
    card4.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 1.2,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  tl.to(
    [card1.value, card2.value, card3.value],
    {
      scale: (index) => 0.9 + index * 0.025,
      yPercent: (index) => -(1.5 - index * 0.375),
      opacity: 0.7,
      duration: 1.5,
      stagger: 0.1,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )
})

onUnmounted(() => {
  if (tl && tl.scrollTrigger) {
    tl.scrollTrigger.kill()
  }
  if (tl) {
    tl.kill()
  }
})
</script>

<style scoped>
/* اطمینان از سفید بودن پس‌زمینه‌ی ورودی‌ها (حتی در حالت autofill مرورگر) */
input {
  background-color: #ffffff !important;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus {
  -webkit-box-shadow: 0 0 0 1000px #ffffff inset !important;
  box-shadow: 0 0 0 1000px #ffffff inset !important;
  -webkit-text-fill-color: #111111 !important;
  caret-color: #111111;
  transition: background-color 5000s ease-in-out 0s;
}

/* Reuse button slide-down effect (same visual as login.vue) */
.btn-slide-down {
  background-color: transparent;
}
.btn-slide-down::before {
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
.btn-slide-down:hover::before,
.btn-slide-down:focus-visible::before {
  transform: translateY(0);
}
.btn-slide-down:hover,
.btn-slide-down:focus-visible {
  color: #ffffff;
  outline: none;
}

/* Card content neutral background matching login page */
.card-content {
  background: linear-gradient(180deg, #f1f2f2 0%, #e8e9e9 100%);
  position: relative;
  z-index: 1;
  color: #808285;
}
/* ensure text sits above accent */
.card-content > * {
  position: relative;
  z-index: 2;
}

/* Vertical yellow accent on the left for desktop, top bar for mobile */
.card-content::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 6px;
  height: 100%;
  background: #FFCD05;
  border-top-left-radius: 50px;
  border-bottom-left-radius: 50px;
  z-index: 1;
}

/* Mobile: convert accent to top thin bar and ensure rounded corners look correct */
@media (max-width: 767px) {
  .card-content::before {
    left: 0;
    top: 0;
    width: 100%;
    height: 6px;
    border-top-left-radius: 50px;
    border-top-right-radius: 50px;
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }
  /* fallback for rounded classes on small screens */
  .rounded-tl-\[50px\], .rounded-bl-\[50px\], .rounded-tr-\[50px\], .rounded-br-\[50px\] {
    border-radius: 50px;
  }
}

/* Slider specific styles */
.slider {
  transition: all 0.6s ease-in-out;
}

.slider.scroll-active {
  /* انیمیشن دلخواه خود را اینجا اضافه کنید */
  animation: slideInUp 0.6s ease-in-out;
}

@keyframes slideInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>