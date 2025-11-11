<template>
    <div>
      <nav class="bg-[#848484] fixed top-0 inset-x-0 z-50 border-b" style="border-bottom-color: #f1f2f2;">
        <div class="max-w-7xl mx-auto">
          <div class="flex items-center h-20">
            <!-- Logo & Brand -->
            <div class="flex items-center justify-start border-r border-l border-white pl-8 pr-8 h-full w-[200px] flex-shrink-0">
                <img src="/english logo W.png" alt="POLYCHEM Logo" class="w-40 h-40 object-contain" />
            </div>
  
            <!-- USE CASES with border and hover effect -->
            <div class="hidden md:flex items-center h-full border-r border-white">
              <button 
                @click="toggleFullscreenMenu"
                class="nav-link flex items-center gap-2 text-[13px] text-white font-medium px-8 h-full text-white relative overflow-hidden"
                :class="{ active: isMenuOpen }"
              >
                <span class="relative z-10">PRODUCTS</span>
                <svg class="w-4 h-4 relative z-10" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"/>
                </svg>
              </button>
            </div>
  
            <!-- Main Menu with hover effects -->
            <div class="hidden md:flex items-center flex-1 justify-center h-full">
              <NuxtLink 
                to="/" 
                class="nav-link text-[13px] font-medium px-8 text-white relative overflow-hidden h-full flex items-center"
              >
                <span class="relative z-10">HOME</span>
              </NuxtLink>
  
              <NuxtLink 
                to="/about" 
                class="nav-link flex items-center justify-center gap-2 text-[13px] font-medium px-8 text-white relative overflow-hidden h-full w-40 "
              >
                <span class="relative z-10">ABOUT US</span>
              </NuxtLink>
  
  
              <NuxtLink 
                to="/applications" 
                class="nav-link text-[14px] font-medium px-8 text-white relative overflow-hidden h-full flex items-center"
              >
                <span class="relative z-10">Applications</span>
              </NuxtLink>
  
              <NuxtLink 
                to="/news" 
                class="nav-link text-[13px] font-medium px-8 text-white relative overflow-hidden h-full flex items-center"
              >
                <span class="relative z-10">NEWS</span>
              </NuxtLink>
  
              <NuxtLink 
                to="/careers" 
                class="nav-link text-[13px] font-medium px-8 text-white relative overflow-hidden h-full flex items-center"
              >
                <span class="relative z-10">CAREERS</span>
              </NuxtLink>
  
              <NuxtLink 
                to="/contact" 
                class="nav-link text-[13px] font-medium px-8 text-white relative overflow-hidden h-full flex items-center w-40"
              >
                <span class="relative z-10">CONTACT US</span>
              </NuxtLink>
            </div>
  
            <!-- Right Side -->
            <div class="hidden md:flex items-center justify-end h-full border-l border-white w-[200px] flex-shrink-0">
              <button class="nav-link text-[13px] font-medium px-8 h-full text-white relative overflow-hidden">
                <span class="relative z-10">EN</span>
              </button>
               <NuxtLink to="/login" class="nav-link text-[13px] font-medium px-8 h-full text-white relative overflow-hidden w-40 border-l border-r border-white flex items-center justify-center">
                  <span class="relative z-10">LOG IN</span>
                </NuxtLink>
            </div>
  
            <!-- Mobile Menu Button -->
            <button 
              @click="mobileMenuOpen = !mobileMenuOpen"
              class="md:hidden text-white hover:text-gray-200 px-4"
            >
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path 
                  v-if="!mobileMenuOpen"
                  stroke-linecap="round" 
                  stroke-linejoin="round" 
                  stroke-width="2" 
                  d="M4 6h16M4 12h16M4 18h16"
                />
                <path 
                  v-else
                  stroke-linecap="round" 
                  stroke-linejoin="round" 
                  stroke-width="2" 
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            </button>
          </div>
  
          <!-- Mobile Menu -->
          <transition name="mobile-menu">
            <div v-if="mobileMenuOpen" class="md:hidden pb-4 border-t border-white/30 mt-2">
              <div class="flex flex-col gap-3 pt-4 px-4">
                <button 
                  @click="toggleFullscreenMenu"
                  class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden text-left"
                  :class="{ active: isMenuOpen }"
                >
                  <span class="relative z-10">USE CASES</span>
                </button>
                <NuxtLink to="/" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">HOME</span>
                </NuxtLink>
                <NuxtLink to="/offers" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden flex items-center gap-2">
                  <span class="relative z-10">OFFERS</span>
                  <span class="text-xs bg-yellow-500 text-white px-1.5 py-0.5 rounded relative z-10">5</span>
                </NuxtLink>
                <NuxtLink to="/about" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">ABOUT</span>
                </NuxtLink>
                <NuxtLink to="/careers" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">CAREERS</span>
                </NuxtLink>
                <NuxtLink to="/jobs" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">JOBS</span>
                </NuxtLink>
                <NuxtLink to="/news" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">NEWS</span>
                </NuxtLink>
                <NuxtLink to="/contact" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded relative overflow-hidden">
                  <span class="relative z-10">CONTACT</span>
                </NuxtLink>
                <div class="border-t border-white/30 pt-3 mt-2">
                  <NuxtLink to="/" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded block mb-2 relative overflow-hidden">
                    <span class="relative z-10">EN</span>
                  </NuxtLink>
                  <NuxtLink to="/report" class="nav-link-mobile text-[13px] font-medium text-white hover:text-gray-200 py-2 px-4 rounded block relative overflow-hidden">
                    <span class="relative z-10">GARTNER REPORT</span>
                  </NuxtLink>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </nav>
      <!-- Spacer to offset fixed navbar height -->
      <div class="h-20"></div>
  
      <!-- Fullscreen Menu -->
      <div 
        ref="fullscreenMenu"
        class="fixed inset-0 z-40 overflow-hidden pointer-events-none"
      >
        <div class="h-full flex">
          <!-- Left Side - Preview Box (40%) -->
          <div class="w-[40%] bg-gradient-to-br from-gray-50 to-white relative overflow-hidden">
            <div 
              ref="previewBox"
              class="absolute inset-0 flex items-center justify-center p-12 opacity-0"
            >
              <div class="text-center space-y-6">
                <div 
                  ref="previewIcon"
                  class="w-32 h-32 mx-auto bg-yellow-400/20 rounded-2xl flex items-center justify-center transform scale-0"
                >
                  <svg class="w-16 h-16 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>
                  </svg>
                </div>
                <div 
                  ref="previewTitle"
                  class="opacity-0 transform translate-y-4"
                >
                  <h3 class="text-3xl font-bold text-gray-900 mb-3">{{ hoveredItem?.title || 'Cyber Solutions' }}</h3>
                  <p class="text-xl text-gray-600">{{ hoveredItem?.code || 'Select a use case' }}</p>
                </div>
                <p 
                  ref="previewDesc"
                  class="text-gray-700 text-lg max-w-md mx-auto opacity-0 transform translate-y-4"
                >
                  {{ hoveredItem?.description || 'Hover over a use case to see details' }}
                </p>
              </div>
  
              <!-- Decorative Grid -->
              <div class="absolute inset-0 opacity-5">
                <div class="grid grid-cols-8 grid-rows-8 h-full w-full">
                  <div v-for="i in 64" :key="i" class="border border-gray-900"></div>
                </div>
              </div>
  
              <!-- Glowing Orb -->
              <div 
                ref="glowingOrb"
                class="absolute w-96 h-96 bg-yellow-400/20 rounded-full blur-3xl opacity-0"
                style="top: 50%; left: 50%; transform: translate(-50%, -50%);"
              ></div>
            </div>
          </div>
  
          <!-- Right Side - Menu Items (60%) -->
          <div class="w-[60%] bg-white relative">
            <div 
              ref="menuContainer"
              class="h-full flex flex-col justify-center px-16 opacity-0 transform translate-y-[-100%]"
            >
              <!-- Close Button -->
              <button 
                @click="toggleFullscreenMenu"
                ref="closeButton"
                class="absolute top-8 right-8 text-gray-700 hover:text-gray-900 transition-colors opacity-0"
              >
                <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
                </svg>
              </button>
  
              <!-- Title -->
              <div 
                ref="menuTitle"
                class="mb-12 mt-16 opacity-0 transform translate-y-[-20px]"
              >
                <h2 class="text-5xl font-bold text-gray-900 mb-2">Use Cases</h2>
                <div class="w-20 h-1 bg-yellow-500"></div>
              </div>
  
              <!-- Menu Items -->
              <div class="space-y-1 max-h-[70vh] overflow-y-auto pr-4 custom-scrollbar">
                <a 
                  v-for="(item, index) in useCases" 
                  :key="index"
                  :ref="el => { if (el) menuItems[index] = el }"
                  href="#"
                  @mouseenter="handleHover(item, index)"
                  @mouseleave="handleLeave"
                  class="menu-item group flex items-center justify-between border-b border-gray-200 py-6 px-6 hover:bg-gray-50 transition-all duration-300 opacity-0 transform translate-y-[-30px]"
                >
                  <div class="flex items-center gap-6">
                    <span class="text-gray-500 text-[13px] font-mono group-hover:text-yellow-500 transition-colors">{{ item.code }}</span>
                    <h3 class="text-gray-900 text-xl font-semibold group-hover:translate-x-2 transition-transform">{{ item.title }}</h3>
                  </div>
                  <svg class="w-6 h-6 text-gray-400 group-hover:text-yellow-500 group-hover:translate-x-2 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"/>
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue'
  import gsap from 'gsap'
  
  const mobileMenuOpen = ref(false)
  const fullscreenMenu = ref(null)
  const menuContainer = ref(null)
  const menuTitle = ref(null)
  const closeButton = ref(null)
  const previewBox = ref(null)
  const previewIcon = ref(null)
  const previewTitle = ref(null)
  const previewDesc = ref(null)
  const glowingOrb = ref(null)
  const menuItems = ref([])
  const isMenuOpen = ref(false)
  const hoveredItem = ref(null)
  
  const useCases = [
    { code: '/ UC_012', title: 'Team awareness', description: 'Enhance collaboration and security awareness across your team with real-time monitoring' },
    { code: '/ UC_005', title: 'Privilege level', description: 'Manage and monitor user access privileges effectively with advanced controls' },
    { code: '/ UC_003', title: 'Cyber regulation', description: 'Stay compliant with latest cybersecurity regulations and industry standards' },
    { code: '/ UC_004', title: 'Data protection', description: 'Protect sensitive data with advanced encryption and security protocols' },
    { code: '/ UC_006', title: 'Third parties exposure', description: 'Monitor and manage third-party security risks and vulnerabilities' },
    { code: '/ UC_007', title: 'Threat Intelligence', description: 'Real-time threat detection and analysis powered by AI' },
    { code: '/ UC_008', title: 'Security reinforcement', description: 'Strengthen your security infrastructure with proactive measures' },
    { code: '/ UC_010', title: 'Security concerns', description: 'Address critical security vulnerabilities before they become threats' },
    { code: '/ UC_011', title: 'Cyber crisis', description: 'Rapid response to cyber security incidents with expert support' },
    { code: '/ UC_000', title: 'Cyber risk assessment', description: 'Comprehensive evaluation of your security posture and risk factors' },
    { code: '/ UC_001', title: 'Test & compare', description: 'Benchmark your security against industry standards and competitors' },
    { code: '/ UC_002', title: 'Cyber disaster readiness', description: 'Prepare for and prevent cyber disasters with disaster recovery plans' }
  ]
  
  const handleHover = (item, index) => {
    hoveredItem.value = item
    
    const tl = gsap.timeline()
    
    // Animate preview content change
    tl.to([previewTitle.value, previewDesc.value], {
      opacity: 0,
      y: -10,
      duration: 0.2
    })
    .set([previewTitle.value, previewDesc.value], {
      opacity: 0
    })
    .to([previewTitle.value, previewDesc.value], {
      opacity: 1,
      y: 0,
      duration: 0.4,
      stagger: 0.1
    })
    
    // Pulse the glowing orb
    gsap.to(glowingOrb.value, {
      scale: 1.2,
      opacity: 0.6,
      duration: 0.6,
      ease: 'power2.out'
    })
  }
  
  const handleLeave = () => {
    gsap.to(glowingOrb.value, {
      scale: 1,
      opacity: 0.3,
      duration: 0.4,
      ease: 'power2.in'
    })
  }
  
  const toggleFullscreenMenu = () => {
    if (!fullscreenMenu.value) return
    
    if (isMenuOpen.value) {
      closeMenu()
    } else {
      openMenu()
    }
    
    isMenuOpen.value = !isMenuOpen.value
  }
  
  const openMenu = () => {
    const menu = fullscreenMenu.value
    
    // Enable pointer events
    menu.style.pointerEvents = 'auto'
    
    // Create timeline
    const tl = gsap.timeline()
    
    // Animate menu container sliding down from top
    tl.to(menuContainer.value, {
      y: 0,
      opacity: 1,
      duration: 0.8,
      ease: 'power3.out'
    })
    
    // Animate title
    tl.to(menuTitle.value, {
      opacity: 1,
      y: 0,
      duration: 0.5,
      ease: 'power2.out'
    }, '-=0.5')
    
    // Animate close button
    tl.to(closeButton.value, {
      opacity: 1,
      duration: 0.3
    }, '-=0.3')
    
    // Animate menu items
    tl.to(menuItems.value, {
      opacity: 1,
      y: 0,
      duration: 0.5,
      ease: 'power2.out',
      stagger: 0.04
    }, '-=0.5')
    
    // Animate preview box
    tl.to(previewBox.value, {
      opacity: 1,
      duration: 0.6
    }, '-=0.8')
    
    tl.to(previewIcon.value, {
      scale: 1,
      duration: 0.6,
      ease: 'back.out(1.7)'
    }, '-=0.4')
    
    tl.to([previewTitle.value, previewDesc.value], {
      opacity: 1,
      y: 0,
      duration: 0.5,
      stagger: 0.1
    }, '-=0.3')
    
    tl.to(glowingOrb.value, {
      opacity: 0.3,
      scale: 1,
      duration: 0.8,
      ease: 'power2.out'
    }, '-=0.6')
  }
  
  const closeMenu = () => {
    const menu = fullscreenMenu.value
    
    // Create timeline
    const tl = gsap.timeline()
    
    // Animate menu items out
    tl.to(menuItems.value, {
      opacity: 0,
      y: -20,
      duration: 0.3,
      ease: 'power2.in',
      stagger: 0.02
    })
    
    // Animate title out
    tl.to(menuTitle.value, {
      opacity: 0,
      y: -20,
      duration: 0.3
    }, '-=0.2')
    
    // Animate preview box out
    tl.to([previewTitle.value, previewDesc.value, previewIcon.value], {
      opacity: 0,
      scale: 0.9,
      duration: 0.3
    }, '-=0.3')
    
    tl.to(glowingOrb.value, {
      opacity: 0,
      duration: 0.3
    }, '-=0.3')
    
    // Animate close button out
    tl.to(closeButton.value, {
      opacity: 0,
      duration: 0.2
    }, '-=0.2')
    
    // Slide menu container up
    tl.to(menuContainer.value, {
      y: '-100%',
      opacity: 0,
      duration: 0.6,
      ease: 'power3.in'
    }, '-=0.2')
    
    tl.to(previewBox.value, {
      opacity: 0,
      duration: 0.3,
      onComplete: () => {
        menu.style.pointerEvents = 'none'
        hoveredItem.value = null
      }
    }, '-=0.4')
  }
  
  onMounted(() => {
    // Set initial state
    gsap.set(menuItems.value, { y: -30, opacity: 0 })
    gsap.set(menuTitle.value, { y: -20, opacity: 0 })
    gsap.set([previewTitle.value, previewDesc.value], { y: 4, opacity: 0 })
    gsap.set(previewIcon.value, { scale: 0 })
    gsap.set(glowingOrb.value, { scale: 0.8, opacity: 0 })
  })
  
  // Close menu on Escape key
  onMounted(() => {
    const handleEscape = (e) => {
      if (e.key === 'Escape' && isMenuOpen.value) {
        toggleFullscreenMenu()
      }
    }
    
    window.addEventListener('keydown', handleEscape)
    
    onUnmounted(() => {
      window.removeEventListener('keydown', handleEscape)
    })
  })
  </script>
  
  <style scoped>
  /* Button 2 Hover Effect - Desktop */
  .nav-link::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    z-index: 0;
    background-color: #000;
    transform: scaleY(0.3);
    opacity: 0;
    transition: all 0.3s ease;
  }
  
  .nav-link:hover::before {
    opacity: 1;
    background-color: #ffcd05;
    transform: scaleY(1);
    transition: transform 0.6s cubic-bezier(0.08, 0.35, 0.13, 1.02), opacity 0.4s;
  }
  
  .nav-link:hover {
    color: #000000 !important;
  }
  
  .nav-link:hover span {
    color: #000000 !important;
  }
  
  /* Mobile Menu Links Hover Effect */
  .nav-link-mobile::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    width: 100%;
    height: 100%;
    z-index: 0;
    background-color: #000;
    transform: scaleY(0.3);
    opacity: 0;
    transition: all 0.3s ease;
  }
  
  .nav-link-mobile:hover::before {
    opacity: 1;
    background-color: #ffcd05;
    transform: scaleY(1);
    transition: transform 0.6s cubic-bezier(0.08, 0.35, 0.13, 1.02), opacity 0.4s;
  }
  
  .nav-link-mobile:hover {
    color: #030303 !important;
  }
  
  .nav-link-mobile:hover span {
    color: #000000 !important;
  }
  
  /* Active state matches hover appearance */
  .nav-link.active::before {
    opacity: 1;
    background-color: #ffffff;
    transform: scaleY(1);
  }
  .nav-link.active,
  .nav-link.active span {
    color: #000000 !important;
  }
  
  .nav-link-mobile.active::before {
    opacity: 1;
    background-color: #ffffff;
    transform: scaleY(1);
  }
  .nav-link-mobile.active,
  .nav-link-mobile.active span {
    color: #030303 !important;
  }
  
  /* Nuxt router active states (desktop) */
  .router-link-active.nav-link::before,
  .router-link-exact-active.nav-link::before {
    opacity: 1;
    background-color: #ffffff;
    transform: scaleY(1);
  }
  .router-link-active.nav-link,
  .router-link-active.nav-link span,
  .router-link-exact-active.nav-link,
  .router-link-exact-active.nav-link span {
    color: #000000 !important;
  }
  
  /* Nuxt router active states (mobile) */
  .router-link-active.nav-link-mobile::before,
  .router-link-exact-active.nav-link-mobile::before {
    opacity: 1;
    background-color: #ffcd05;
    transform: scaleY(1);
  }
  .router-link-active.nav-link-mobile,
  .router-link-active.nav-link-mobile span,
  .router-link-exact-active.nav-link-mobile,
  .router-link-exact-active.nav-link-mobile span {
    color: #030303 !important;
  }
  
  /* Mobile Menu Animation */
  .mobile-menu-enter-active,
  .mobile-menu-leave-active {
    transition: all 0.3s ease;
  }
  
  .mobile-menu-enter-from {
    opacity: 0;
    transform: translateY(-10px);
  }
  
  .mobile-menu-leave-to {
    opacity: 0;
    transform: translateY(-10px);
  }
  
  /* Menu Item Hover Effect */
  .menu-item {
    transform: translateZ(0);
    backface-visibility: hidden;
  }
  
  /* Custom Scrollbar */
  .custom-scrollbar::-webkit-scrollbar {
    width: 6px;
  }
  
  .custom-scrollbar::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.05);
    border-radius: 3px;
  }
  
  .custom-scrollbar::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 3px;
  }
  
  .custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background: rgba(234, 179, 8, 0.5);
  }
  </style>