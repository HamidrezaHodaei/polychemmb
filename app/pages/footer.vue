<template>
  <footer class="bg-[#1e1e1e] text-gray-300 relative">
    <!-- Main Footer Content -->
    <div class="max-w-7xl mx-auto px-6 py-16">
      <!-- Top Section with Columns -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 mb-12">
        <!-- Column 1: Brand -->
        <div class="space-y-4">
          <h2 class="text-3xl font-serif text-white mb-2" style="font-family: 'Playfair Display', serif;">
            YourBrand
          </h2>
          <p class="text-sm text-gray-400 leading-relaxed">
            Crafting exceptional digital experiences with elegance and innovation.
          </p>
          
          <!-- Language Switcher -->
          <div class="relative pt-4">
            <button
              @click="toggleLanguageDropdown"
              class="flex items-center gap-2 px-4 py-2 bg-[#2a2a2a] rounded-lg border border-[#848484]/30 hover:border-[#d4af37] transition-all duration-300 w-full justify-between"
              aria-label="Select language"
            >
              <div class="flex items-center gap-2">
                <svg class="w-4 h-4 text-[#d4af37]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3.055 11H5a2 2 0 012 2v1a2 2 0 002 2 2 2 0 012 2v2.945M8 3.935V5.5A2.5 2.5 0 0010.5 8h.5a2 2 0 012 2 2 2 0 104 0 2 2 0 012-2h1.064M15 20.488V18a2 2 0 012-2h3.064M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <span class="text-sm">{{ currentLanguage }}</span>
              </div>
              <svg class="w-4 h-4 transition-transform duration-300" :class="{ 'rotate-180': languageDropdownOpen }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
              </svg>
            </button>
            
            <!-- Dropdown -->
            <div
              v-show="languageDropdownOpen"
              class="absolute bottom-full mb-2 w-full bg-[#2a2a2a] border border-[#848484]/30 rounded-lg overflow-hidden shadow-xl z-10"
            >
              <button
                @click="selectLanguage('English')"
                class="w-full px-4 py-3 text-left text-sm hover:bg-[#d4af37]/10 hover:text-[#d4af37] transition-colors duration-200 flex items-center gap-2"
              >
                <span class="text-lg">🇬🇧</span>
                <span>English</span>
              </button>
              <button
                @click="selectLanguage('فارسی')"
                class="w-full px-4 py-3 text-left text-sm hover:bg-[#d4af37]/10 hover:text-[#d4af37] transition-colors duration-200 flex items-center gap-2"
              >
                <span class="text-lg">🇮🇷</span>
                <span>فارسی</span>
              </button>
            </div>
          </div>
        </div>

        <!-- Column 2: Quick Links -->
        <div>
          <h3 class="text-white font-semibold text-lg mb-6 relative inline-block">
            Quick Links
            <span class="absolute bottom-0 left-0 w-12 h-0.5 bg-[#d4af37]"></span>
          </h3>
          <ul class="space-y-3">
            <li v-for="link in quickLinks" :key="link">
              <a
                href="#"
                class="text-gray-400 hover:text-[#d4af37] transition-colors duration-300 text-sm inline-block hover:translate-x-1 transform"
              >
                {{ link }}
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 3: Resources -->
        <div>
          <h3 class="text-white font-semibold text-lg mb-6 relative inline-block">
            Resources
            <span class="absolute bottom-0 left-0 w-12 h-0.5 bg-[#d4af37]"></span>
          </h3>
          <ul class="space-y-3">
            <li v-for="resource in resources" :key="resource">
              <a
                href="#"
                class="text-gray-400 hover:text-[#d4af37] transition-colors duration-300 text-sm inline-block hover:translate-x-1 transform"
              >
                {{ resource }}
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 4: Social & Newsletter -->
        <div>
          <h3 class="text-white font-semibold text-lg mb-6 relative inline-block">
            Connect With Us
            <span class="absolute bottom-0 left-0 w-12 h-0.5 bg-[#d4af37]"></span>
          </h3>
          
          <!-- Social Icons -->
          <div class="flex flex-wrap gap-3 mb-6">
            <a
              v-for="social in socialLinks"
              :key="social.name"
              :href="social.url"
              :aria-label="social.name"
              class="w-10 h-10 rounded-full bg-[#2a2a2a] flex items-center justify-center text-gray-400 hover:text-[#d4af37] hover:bg-[#d4af37]/10 hover:scale-110 transition-all duration-300 border border-transparent hover:border-[#d4af37]/50"
            >
              <component :is="social.icon" class="w-5 h-5" />
            </a>
          </div>

          <!-- Newsletter -->
          <div class="space-y-3">
            <p class="text-sm text-gray-400">Subscribe to our newsletter</p>
            <div class="flex gap-2">
              <input
                v-model="email"
                type="email"
                placeholder="Your email"
                class="flex-1 px-4 py-2 bg-[#2a2a2a] border border-[#848484]/30 rounded-lg text-sm text-white placeholder-gray-500 focus:outline-none focus:border-[#d4af37] transition-colors duration-300"
              />
              <button
                @click="subscribe"
                class="px-6 py-2 bg-[#d4af37] text-black font-semibold rounded-lg hover:bg-[#e6c567] transition-all duration-300 hover:scale-105 text-sm"
                aria-label="Subscribe to newsletter"
              >
                Subscribe
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Separator -->
      <hr class="border-t border-[#d4af37]/30 mb-8" />

      <!-- Partners Section -->
      <div class="mb-8">
        <h4 class="text-white text-center text-sm font-semibold mb-6 tracking-wider">OUR PARTNERS</h4>
        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-6 items-center">
          <div
            v-for="i in 6"
            :key="i"
            class="flex items-center justify-center p-4 bg-[#2a2a2a] rounded-lg grayscale hover:grayscale-0 hover:bg-[#d4af37]/10 transition-all duration-300 cursor-pointer group"
          >
            <span class="text-gray-600 group-hover:text-[#d4af37] font-semibold text-xs transition-colors duration-300">
              PARTNER {{ i }}
            </span>
          </div>
        </div>
      </div>

      <!-- Separator -->
      <hr class="border-t border-[#d4af37]/30 mb-6" />

      <!-- Bottom Copyright -->
      <div class="text-center text-sm text-gray-500">
        <p>
          Copyright © 2025 Your Company. All rights reserved. 
          <span class="mx-2">|</span>
          <span class="inline-flex items-center gap-1">
            ساخته شده با 
            <span class="text-red-500 animate-pulse">❤️</span>
          </span>
        </p>
      </div>
    </div>

    <!-- Back to Top Button -->
    <button
      v-show="showBackToTop"
      @click="scrollToTop"
      class="fixed bottom-8 right-8 w-12 h-12 rounded-full bg-[#1e1e1e] border-2 border-[#d4af37] text-white flex items-center justify-center hover:bg-[#d4af37] hover:scale-110 transition-all duration-300 shadow-lg z-50"
      aria-label="Back to top"
    >
      <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 10l7-7m0 0l7 7m-7-7v18" />
      </svg>
    </button>
  </footer>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Data
const email = ref('')
const showBackToTop = ref(false)
const languageDropdownOpen = ref(false)
const currentLanguage = ref('English')

const quickLinks = ['Home', 'About', 'Services', 'Portfolio', 'Blog', 'Contact']
const resources = ['Support', 'FAQ', 'Privacy Policy', 'Terms', 'Careers']

const socialLinks = [
  {
    name: 'Facebook',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>`
    }
  },
  {
    name: 'Twitter',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>`
    }
  },
  {
    name: 'Instagram',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>`
    }
  },
  {
    name: 'LinkedIn',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>`
    }
  },
  {
    name: 'YouTube',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>`
    }
  },
  {
    name: 'Telegram',
    url: '#',
    icon: {
      template: `<svg fill="currentColor" viewBox="0 0 24 24"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.48.33-.913.49-1.302.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>`
    }
  }
]

// Methods
const subscribe = () => {
  if (email.value) {
    alert(`Subscribed with: ${email.value}`)
    email.value = ''
  }
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const handleScroll = () => {
  showBackToTop.value = window.scrollY > 300
}

const toggleLanguageDropdown = () => {
  languageDropdownOpen.value = !languageDropdownOpen.value
}

const selectLanguage = (lang) => {
  currentLanguage.value = lang
  languageDropdownOpen.value = false
}

// Lifecycle
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  document.addEventListener('click', (e) => {
    if (!e.target.closest('.relative')) {
      languageDropdownOpen.value = false
    }
  })
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&display=swap');

/* Custom animations */
@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>