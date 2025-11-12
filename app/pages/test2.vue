<template>
  <!-- 
    کامپوننت Nuxt 3 با استفاده از Tailwind CSS و GSAP
    تبدیل از HTML/CSS/JS به Vue 3 Composition API
    
    تغییرات اصلی:
    1. تمام استایل‌ها با Tailwind جایگزین شدند
    2. انیمیشن‌ها با GSAP Timeline بهینه شدند
    3. از ScrollTrigger برای پین کردن و اسکرول استفاده شد
    4. ساختار Vue 3 <script setup> برای راحتی و بهینه‌سازی
  -->
  <div class="container mx-auto px-4">
    <div class="w-full">
      <!-- Cards Container: نسبت به viewport پین می‌شود و 70vh ارتفاع دارد -->
      <div 
        ref="cardsContainer" 
        class="relative flex justify-center items-center h-[70vh] mb-24"
      >
        <!-- Card 1: پایه‌ای‌ترین کارت با z-index کمتر -->
        <div
          ref="card1"
          class="absolute w-4/5 h-[70vh] rounded-[50px] shadow-2xl flex justify-center items-center text-white text-4xl font-bold"
          style="
            top: 0px;
            z-index: 2;
            background: linear-gradient(-40deg, #d754ad 0%, #f96785 67%, #fe7333 100%);
          "
        >
          <h1>Slide 1</h1>
        </div>

        <!-- Card 2: 30px پایین‌تر از Card 1 -->
        <div
          ref="card2"
          class="absolute w-4/5 h-[70vh] rounded-[50px] shadow-2xl flex justify-center items-center text-white text-4xl font-bold"
          style="
            top: 30px;
            z-index: 3;
            background: linear-gradient(-40deg, #4facfe 0%, #00f2fe 100%);
          "
        >
          <h1>Slide 2</h1>
        </div>

        <!-- Card 3: 60px پایین‌تر -->
        <div
          ref="card3"
          class="absolute w-4/5 h-[70vh] rounded-[50px] shadow-2xl flex justify-center items-center text-white text-4xl font-bold"
          style="
            top: 60px;
            z-index: 4;
            background: linear-gradient(-40deg, #43e97b 0%, #38f9d7 100%);
          "
        >
          <h1>Slide 3</h1>
        </div>

        <!-- Card 4: 90px پایین‌تر - بالاترین لایه -->
        <div
          ref="card4"
          class="absolute w-4/5 h-[70vh] rounded-[50px] shadow-2xl flex justify-center items-center text-white text-4xl font-bold"
          style="
            top: 90px;
            z-index: 5;
            background: linear-gradient(-40deg, #fa709a 0%, #fee140 100%);
          "
        >
          <h1>Slide 4</h1>
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

// ثبت پلاگین‌های GSAP
gsap.registerPlugin(ScrollTrigger, ScrollToPlugin)

// refs برای المنت‌های DOM
const cardsContainer = ref(null)
const card1 = ref(null)
const card2 = ref(null)
const card3 = ref(null)
const card4 = ref(null)

let tl = null

onMounted(() => {
  // ایجاد یک Timeline اصلی با ScrollTrigger
  // بهبود: استفاده از ease برای انیمیشن‌های نرم‌تر
  tl = gsap.timeline({
    scrollTrigger: {
      trigger: cardsContainer.value,
      pin: true, // پین کردن کانتینر هنگام اسکرول
      pinSpacing: true,
      markers: false, // برای دیباگ می‌توان true کرد
      start: 'top-=50px top',
      end: '+=2000', // 2000px اسکرول برای تمام انیمیشن
      scrub: 1, // انیمیشن با اسکرول همگام می‌شود (1 ثانیه تاخیر برای نرمی)
    },
  })

  // --- انیمیشن Card 1: شروع با opacity کامل ---
  tl.addLabel('card1')
  tl.to(card1.value, {
    yPercent: 0,
    opacity: 1,
    duration: 0.5,
    ease: 'power2.out',
  })

  // --- انیمیشن Card 2: ورود از پایین ---
  tl.from(
    card2.value,
    {
      yPercent: 75, // 75% از ارتفاع خودش پایین‌تر
      opacity: 0,
      duration: 0.8,
      ease: 'power2.out',
    },
    '+=0.2' // تاخیر کوچک برای روانی
  )

  tl.addLabel('card2')

  // بهبود: انیمیشن همزمان Card 1 و Card 2 برای تاثیر بهتر
  tl.to(
    card1.value,
    {
      scale: 0.925, // کوچک‌تر شدن
      yPercent: -0.75, // کمی بالا رفتن
      opacity: 0.85, // کمی محو شدن
      duration: 0.6,
      ease: 'power2.inOut',
    },
    '-=0.4' // همپوشانی با انیمیشن قبل
  )

  tl.to(
    card2.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 0.6,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  // --- انیمیشن Card 3: ورود از پایین ---
  tl.from(
    card3.value,
    {
      yPercent: 75,
      opacity: 0,
      duration: 0.8,
      ease: 'power2.out',
    },
    '+=0.2'
  )

  tl.addLabel('card3')

  // Card 2 کوچک‌تر می‌شود و Card 3 جای آن را می‌گیرد
  tl.to(
    card2.value,
    {
      scale: 0.95,
      yPercent: -0.5,
      opacity: 0.85,
      duration: 0.6,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )

  tl.to(
    card3.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 0.6,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  // --- انیمیشن Card 4: ورود از پایین ---
  tl.from(
    card4.value,
    {
      yPercent: 75,
      opacity: 0,
      duration: 0.8,
      ease: 'power2.out',
    },
    '+=0.2'
  )

  tl.addLabel('card4')

  // Card 3 کوچک‌تر می‌شود و Card 4 جای آن را می‌گیرد
  tl.to(
    card3.value,
    {
      scale: 0.975,
      yPercent: -0.4,
      opacity: 0.85,
      duration: 0.6,
      ease: 'power2.inOut',
    },
    '-=0.4'
  )

  tl.to(
    card4.value,
    {
      yPercent: 0,
      opacity: 1,
      duration: 0.6,
      ease: 'power2.out',
    },
    '-=0.6'
  )

  // --- بهبود: انیمیشن‌های نهایی برای تمام کارت‌ها با stagger ---
  // تمام کارت‌های قبلی را به صورت هماهنگ کوچک‌تر و محو می‌کنیم
  tl.to(
    [card1.value, card2.value, card3.value],
    {
      scale: (index) => 0.9 + index * 0.025, // هر کدام مقداری متفاوت
      yPercent: (index) => -(1.5 - index * 0.375),
      opacity: 0.7,
      duration: 0.8,
      stagger: 0.1, // تاخیر بین هر المنت برای جذابیت بیشتر
      ease: 'power2.inOut',
    },
    '-=0.4'
  )
})

// پاک کردن ScrollTrigger هنگام از بین رفتن کامپوننت
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
/* 
  استایل‌های سفارشی فقط برای چیزهایی که Tailwind پوشش نمی‌دهد
  در اینجا از Tailwind برای تمام استایل‌ها استفاده شده است
*/
</style>