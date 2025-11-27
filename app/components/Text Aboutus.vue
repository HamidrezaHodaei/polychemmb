<template>
  <section class="w-full py-8 flex items-center justify-center bg-[#f1f2f2]">
    <div class="flex flex-col sm:flex-row items-center justify-center text-center sm:text-left px-6 gap-8 max-w-[1200px] w-full">

      <!-- متن ثابت - همه در یک خط -->
      <p class="font-bold text-[#848484] uppercase">
        <span class="brand-rest">Where every grain of polymer becomes</span>
      </p>

      <!-- باکس انیمیشنی -->
      <div class="h-[60px] overflow-hidden">
        <div class="animate-slide">
          <div class="box">Polymer Art</div>
          <div class="box">Originality</div>
          <div class="box">Loyalty</div>
          <div class="box">Creativity</div>
          <div class="box">Honesty</div>
          <div class="box">Elegance</div>
          <div class="box">Meaningful</div>
          <div class="box">Polymer Art</div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700;800&display=swap');

* {
  font-family: 'Montserrat', sans-serif;
}

/* Polychem دقیقاً ۴۰px و هم‌تراز با بقیه */
.brand {
  font-weight: 800;
  font-size: 45px;
  letter-spacing: 0.05em;
  white-space: nowrap;           /* مهم: نمی‌ذاره بشکنه */
  display: inline-block;
  vertical-align: middle;
}

/* خط تیره بینشون */
.brand-separator {
  font-weight: 700;
  font-size: 36px;
  margin: 0 0.6rem;
  opacity: 0.9;
  vertical-align: middle;
}

/* متن ادامه‌دهنده - هم‌سایز و هم‌تراز و در یک خط */
.brand-rest {
  font-weight: 700;
  font-size: 35px;               /* حالا هم‌سایز Polychem */
  white-space: nowrap;
  display: inline-block;
  vertical-align: middle;
}

/* باکس‌های زرد */
.box {
  height: 60px;
  background: #FFD700;
  color: white;
  font-weight: 800;
  font-size: 35px;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 2rem;
  white-space: nowrap;
}

/* انیمیشن روان */
.animate-slide {
  animation: slide 21s infinite linear;
}

@keyframes slide {
  0%, 11%    { transform: translateY(0); }
  12.5%, 23.5% { transform: translateY(-60px); }
  25%, 36%   { transform: translateY(-120px); }
  37.5%, 48.5% { transform: translateY(-180px); }
  50%, 61%   { transform: translateY(-240px); }
  62.5%, 73.5% { transform: translateY(-300px); }
  75%, 86%   { transform: translateY(-360px); }
  87.5%, 98.5% { transform: translateY(-420px); }
  100%       { transform: translateY(0); }
}

/* موبایل: اگر صفحه خیلی کوچیک شد، فقط کمی فونت رو کوچیک می‌کنیم (ولی همچنان یک خط) */
@media (max-width: 540px) {
  .brand,
  .brand-rest,
  .brand-separator {
    font-size: 32px;
  }
  .brand-separator {
    font-size: 30px;
    margin: 0 0.4rem;
  }
  .box {
    font-size: 32px;
    padding: 0 1.5rem;
  }
}
</style>