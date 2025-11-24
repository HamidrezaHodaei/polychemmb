<!-- Footer Component - Fully Responsive -->
<template>
  <footer class="relative w-full bg-[#848484] text-white overflow-hidden font-mono">
    <!-- Decorative Top Border -->
    <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-transparent via-white to-transparent opacity-30"></div>
    
    <!-- Main Footer Content -->
    <div class="max-w-[1400px] mx-auto px-4 sm:px-6 md:px-8 py-12 sm:py-16 md:py-20">
      <!-- Top Section -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 sm:gap-10 md:gap-12 lg:gap-16 mb-12 sm:mb-14 md:mb-16">
        <!-- Brand Section -->
        <div class="space-y-4 sm:space-y-5 md:space-y-6 sm:col-span-2 lg:col-span-1">
          <h2 class="text-2xl sm:text-3xl md:text-4xl font-mono tracking-tight">Polychem</h2>
          <p class="text-white leading-relaxed text-sm sm:text-base max-w-sm">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua
          </p>
          <!-- Social Links -->
          <div class="flex gap-3 sm:gap-4 pt-2 sm:pt-4">
            <!-- YouTube Icon (replaces GitHub) -->
            <a 
              href="#" 
              class="youtube-icon w-9 h-9 sm:w-10 sm:h-10 rounded-full border border-white flex items-center justify-center hover:border-white hover:bg-white hover:text-black transition-all duration-300 active:scale-95"
              aria-label="YouTube"
            >
              <svg class="w-4 h-4 sm:w-5 sm:h-5 youtube-svg" fill="white" viewBox="0 0 24 24">
                <path d="M23.498 6.186a2.994 2.994 0 0 0-2.108-2.117C19.633 3.5 12 3.5 12 3.5s-7.633 0-9.39.569A2.994 2.994 0 0 0 .502 6.186C0 7.943 0 12 0 12s0 4.057.502 5.814a2.994 2.994 0 0 0 2.108 2.117C4.367 20.5 12 20.5 12 20.5s7.633 0 9.39-.569a2.994 2.994 0 0 0 2.108-2.117C24 16.057 24 12 24 12s0-4.057-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
              </svg>
            </a>
            <a 
              href="#" 
              class="w-9 h-9 sm:w-10 sm:h-10 rounded-full border border-white flex items-center justify-center hover:border-white hover:bg-white hover:text-black transition-all duration-300 active:scale-95"
              aria-label="LinkedIn"
            >
              <svg class="w-4 h-4 sm:w-5 sm:h-5" fill="currentColor" viewBox="0 0 24 24">
                <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
              </svg>
            </a>
            <a 
              href="mailto:Hodaeiihamidreza@gmail.com" 
              class="w-9 h-9 sm:w-10 sm:h-10 rounded-full border border-white flex items-center justify-center hover:border-white hover:bg-white hover:text-black transition-all duration-300 active:scale-95"
              aria-label="Email"
            >
              <svg class="w-4 h-4 sm:w-5 sm:h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
              </svg>
            </a>
          </div>
        </div>

        <!-- Navigation -->
        <div class="sm:flex sm:justify-center lg:justify-center">
          <div>
            <h3 class="text-base sm:text-lg font-semibold mb-4 sm:mb-5 md:mb-6 uppercase tracking-wider">Navigation</h3>
            <ul class="space-y-2 sm:space-y-3">
              <li><a href="/" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">HOME</a></li>
              <li><a href="/about" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">ABOUT US</a></li>
              <li><a href="/applications" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">Applications</a></li>
              <li><a href="/news" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">NEWS</a></li>
              <li><a href="/careers" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">CAREERS</a></li>
              <li><a href="/contact" class="text-white hover:text-white transition-colors duration-300 text-sm sm:text-base inline-block">CONTACT US</a></li>
            </ul>
          </div>
        </div>

        <!-- Contact -->
        <div class="sm:col-span-2 lg:col-span-1">
          <h3 class="text-base sm:text-lg font-semibold mb-4 sm:mb-5 md:mb-6 uppercase tracking-wider">Contact</h3>
          <ul class="space-y-3 sm:space-y-4">
            <li class="flex items-start gap-2 sm:gap-3 text-white text-sm sm:text-base">
              <svg class="w-4 h-4 sm:w-5 sm:h-5 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
              </svg>
              <span>Iran, Tehran</span>
            </li>
            <li class="flex items-start gap-2 sm:gap-3 text-white text-sm sm:text-base">
              <svg class="w-4 h-4 sm:w-5 sm:h-5 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
              </svg>
              <a href="mailto:info@polychemmb.com" class="hover:text-white transition-colors break-all">
                info@polychemmb.com
              </a>
            </li>
          </ul>
        </div>
      </div>

      <!-- Newsletter Section -->
      <div class="border-t border-b border-white py-8 sm:py-10 md:py-12">
        <div class="max-w-2xl mx-auto text-center">
          <h3 class="text-xl sm:text-2xl font-semibold mb-2 sm:mb-3">Let's Work Together</h3>
          <p class="text-white mb-5 sm:mb-6 text-sm sm:text-base px-4">Get notified about new projects and professional updates</p>
          <form class="flex flex-col sm:flex-row gap-3 max-w-md mx-auto px-4">
            <input 
              type="email" 
              placeholder="Your email address"
              class="flex-1 px-4 sm:px-5 py-2.5 sm:py-3 bg-white/5 border border-white rounded-lg focus:outline-none focus:border-white transition-colors text-sm sm:text-base"
            >
            <button type="submit" class="card-button">
              <span>Notify Me</span>
            </button>
          </form>
        </div>
      </div>

      <!-- Bottom Section -->
      <div class="pt-8 sm:pt-10 md:pt-12 flex flex-col md:flex-row justify-between items-center gap-4 sm:gap-6">
        <p class="text-white text-xs sm:text-sm text-center md:text-left order-2 md:order-1">
          © 2025 Polychem. All rights reserved.
        </p>
        <div class="flex flex-wrap justify-center gap-3 sm:gap-6 text-xs sm:text-sm order-1 md:order-2">
          <a href="#" class="text-white hover:text-white transition-colors whitespace-nowrap">Legal Notice</a>
          <span class="text-white">|</span>
          <a href="#" class="text-white hover:text-white transition-colors whitespace-nowrap">Cookie Policy</a>
          <span class="text-white">|</span>
          <a href="#" class="text-white hover:text-white transition-colors whitespace-nowrap">Privacy</a>
        </div>
      </div>
    </div>

    <!-- Decorative Bottom Element -->
    <div class="absolute bottom-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-white/20 to-transparent"></div>
  </footer>
</template>

<script setup>
// No script needed for this component
</script>

<style scoped>
/* تغییر: ایمپورت فونت‌ها مشابه Hero */
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&family=Figtree:wght@300;400;500;600;700&display=swap');

/* تنظیم فونت پیش‌فرض footer مطابق Hero (متن -> Figtree، عناوین -> Montserrat) */
footer {
  font-family: 'Figtree', sans-serif;
}

/* عناوین داخل footer مشابه Hero */
footer h2,
footer h3,
footer .text-2xl,
footer .text-xl,
footer .font-semibold {
  font-family: 'Montserrat', sans-serif;
  /* در صورت نیاز وزن دلخواه را اینجا اضافه کنید */
}

/* Smooth transitions for all interactive elements */
a, button {
  -webkit-tap-highlight-color: transparent;
}

/* دکمه مشابه cards.vue */
.card-button {
  background: transparent;
  color: #FFCD05;
  border: 2px solid #FFCD05;
  padding: 12px 28px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  z-index: 1;
  transition: color 200ms ease;
}

.card-button::before {
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

.card-button:hover::before,
.card-button:focus-visible::before {
  transform: translateY(0);
}

.card-button:hover,
.card-button:focus-visible {
  color: #ffffff;
  outline: none;
}

.card-button span {
  position: relative;
  z-index: 1;
}

/* YouTube icon hover: svg fill to black */
.youtube-icon:hover .youtube-svg {
  fill: #000 !important;
}
</style>