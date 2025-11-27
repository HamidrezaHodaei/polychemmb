<template>
  <div class="h-screen bg-[#f1f2f2] flex items-center justify-center bg-cover bg-center bg-blend-overlay" style="background-image: url('https://images.unsplash.com/photo-1598322508096-5ca34544d8aa?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80')">
    <div 
      ref="containerRef"
      :class="['w-full h-full flex overflow-x-auto overflow-y-hidden scroll-smooth bg-[#ffffff] shadow-2xl', {'detail-mode': activeProductIndex !== null}]"
    >
      <!-- Navigation Sidebar -->
      <nav class="w-[60px] lg:w-[100px] bg-[#848484] flex flex-col items-center justify-between py-7 px-4 text-white flex-shrink-0 relative z-10">
        <!-- back/home icon -->
        <div class="w-8 text-yellow-600">
          <NuxtLink to="/" aria-label="Go to home" class="block p-1">
            <!-- خانه (home) icon -->
            <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10.5L12 4l9 6.5V20a1 1 0 01-1 1h-5v-6H9v6H4a1 1 0 01-1-1V10.5z"/>
            </svg>
          </NuxtLink>
        </div>
        
        <div class="hidden lg:block [writing-mode:vertical-rl] -rotate-90">
          <NuxtLink to="/" aria-label="Go to home" class="block">
            <img src="/english logo W1.png" alt="Logo" class="w-28 h-auto object-contain">
          </NuxtLink>
        </div>
        
        <div class="relative flex items-center justify-center flex-col">
          <!-- لینک لاگین در پایین‌ترین بخش نوار کناری -->
          <NuxtLink to="/login" aria-label="Log in" class="block p-1 mt-2">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 20v-1a4 4 0 00-4-4H9a4 4 0 00-4 4v1" />
            </svg>
          </NuxtLink>
        </div>
      </nav>

      <!-- Cover Image -->
      <div class="w-[300px] lg:w-[500px] flex items-center justify-center text-5xl text-white font-medium text-center flex-shrink-0 bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1598322508096-5ca34544d8aa?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80')">
       
      </div>

      <!-- Products -->
      <div
        v-for="(product, index) in products"
        :key="index"
        :data-index="index"
        @click="handleProductClick(index, $event)"
        :class="['flex flex-col items-center p-9 bg-[#f1f2f2] w-[310px] overflow-y-auto scroll-smooth transition-all duration-500 flex-shrink-0 ml-1.5 relative', {'product-active w-full lg:w-[70%] px-8 lg:px-[70px] pb-0': activeProductIndex === index, 'hover:shadow-[inset_0_-4px_0_0_#FFCD05] cursor-pointer': activeProductIndex !== index}]"
      >
        <!-- Close Button -->
        <button
          v-if="activeProductIndex === index"
          @click.stop="closeProduct"
          class="sticky top-0 ml-auto -mr-10 lg:-mr-10 w-9 h-9 rounded-full bg-black/50 flex items-center justify-center text-white z-10 flex-shrink-0 mb-4"
        >
          <svg class="w-6 h-6" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>

        <!-- Brand -->
        <div :class="['text-sm tracking-widest transition-all duration-500', activeProductIndex === index && 'text-base']">
          {{ product.brand }}
        </div>

        <!-- Title -->
        <h2 :class="['text-center font-medium my-5 transition-all duration-500', activeProductIndex === index ? 'text-3xl lg:text-[44px] max-w-none' : 'text-xl max-w-[12ch]']">
          {{ product.title }}
        </h2>

        <!-- Price -->
        <div v-if="product.price" :class="['mb-8 transition-all duration-500', activeProductIndex === index && 'text-2xl']">
          {{ product.price }}
        </div>

        <!-- Buttons (shown when active) -->
        <div v-if="activeProductIndex === index" class="flex items-center min-w-[80%] mb-9 animate-fade-up flex-col lg:flex-row" style="animation-delay: 0.2s">
          <button class="btn-slide-down w-full lg:w-auto h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors px-5 py-3 text-sm font-medium tracking-widest flex-grow rounded ml-0 lg:ml-4 mt-4 lg:mt-0">
            <span class="relative z-10">ADD TO CART</span>
          </button>
        </div>

        <!-- Subtitle -->
        <p v-if="activeProductIndex !== index" class="text-gray-700 leading-relaxed text-sm mb-5 whitespace-pre-line">
          {{ product.subtitle }}
        </p>

        <!-- Expanded Content (shown when active) -->
        <template v-if="activeProductIndex === index">
          <p class="text-gray-700 leading-relaxed text-sm mb-5 animate-fade-up whitespace-pre-line" style="animation-delay: 0.3s">
            {{ product.subtitle }}
          </p>
          
          <img 
            src="/955B.jpg"
            alt="Detail"
            class="block mt-5 -mx-8 lg:-mx-[70px] max-w-none w-[calc(100%+64px)] lg:w-[calc(100%+140px)]"
          />

          <!-- Technical Specifications Table -->
          <div class="flex flex-col bg-[#A8A8A8] text-white -mx-8 lg:-mx-[70px] px-8 lg:px-[100px] py-12 lg:py-[70px] w-[calc(100%+64px)] lg:w-[calc(100%+140px)]">
            <h3 class="font-semibold tracking-widest text-2xl mb-8 text-[#FFCD05]">TECHNICAL SPECIFICATIONS</h3>
            
            <div v-for="(spec, idx) in product.specs" :key="idx" class="flex flex-col lg:flex-row pt-4 mt-4 border-t border-white/20">
              <div class="flex-shrink-0 font-semibold tracking-wide w-full lg:w-[280px] text-[#FFCD05] mb-2 lg:mb-0">{{ spec.label }}</div>
              <div class="leading-relaxed text-sm text-white/90">{{ spec.value }}</div>
            </div>
          </div>

          <!-- Image Grid -->
          <div class="flex flex-wrap -mx-8 lg:-mx-[70px] w-[calc(100%+64px)] lg:w-[calc(100%+140px)]">
            <img src="https://images.unsplash.com/photo-1596646912242-80d82d06c463?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80" alt="" class="w-1/3 max-h-80 object-cover">
            <img src="https://images.unsplash.com/photo-1594649537448-8133756764c0?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=975&q=80" alt="" class="w-1/3 max-h-80 object-cover">
            <img src="https://images.unsplash.com/photo-1582147037191-10d00acdf2d8?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80" alt="" class="w-1/3 max-h-80 object-cover">
            <img src="https://images.unsplash.com/photo-1589100984317-79246528923c?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=2089&q=80" alt="" class="w-1/2 object-cover">
            <img src="https://images.unsplash.com/photo-1571068299107-cd63d456bf5d?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1567&q=80" alt="" class="w-1/2 object-cover">
          </div>
        </template>

        <!-- View Details (shown when not active) -->
        <button v-else class="mt-auto font-medium text-sm tracking-widest text-[#FFCD05] hover:text-[#e6b800] transition-colors">VIEW DETAILS →</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick } from 'vue';

const containerRef = ref(null);
const activeProductIndex = ref(null);

const products = [
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F700',
    price: '',
    subtitle: `POLYFIL F700 is a high-performance polyethylene compound specifically formulated for high-density polyethylene (HDPE) blown film applications. This grade is engineered to deliver superior mechanical properties, excellent film uniformity, and reliable processability, even in ultra-thin film applications.
It is highly recommended for producing films with thicknesses in the range of 10–25 microns, making it suitable for a wide range of packaging and consumer products such as shopping bags, T-shirt bags, garbage bags, liner bags, and food-contact films.`,
    specs: [
      { label: 'MFI (190°C / 5 kg)', value: '0.19 ± 0.1 g/10min (ISO 1133)' },
      { label: 'Density', value: '0.952 ± 0.003 kg/m³ (ISO 1183)' },
      { label: 'Moisture Content', value: '≤ 1500 ppm' },
      { label: 'Melting Point', value: '135 ± 5°C (ASTM D 2117)' },
      { label: 'Vicat Softening Point', value: '124°C (ASTM D1525)' },
      { label: 'Tensile Strength @ Yield (MD)', value: '24 MPa (ASTM D638)' },
      { label: 'Tensile Strength @ Yield (MD, TD)', value: '30, 60 MPa (ASTM D638)' },
      { label: 'Elongation @ Break (MD, TD)', value: '450, 240 % (ASTM D638)' },
      { label: 'ESCR (IGEPAL 10%) (F50, 50°C)', value: '> 1000 HR (ASTM D1693)' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F800',
    price: '',
    subtitle: 'Advanced polyethylene compound designed for superior film processing with enhanced mechanical strength and clarity for demanding packaging applications.',
    specs: [
      { label: 'FRAME', value: 'Carbon fiber composite' },
      { label: 'SEAT BINDER', value: 'Aluminum alloy' },
      { label: 'REAR SHOCK', value: 'High-performance air suspension' },
      { label: 'FORK', value: 'Adjustable travel fork system' },
      { label: 'STEM', value: 'CNC machined aluminum' },
      { label: 'HANDLEBARS', value: 'Carbon fiber reinforced' },
      { label: 'GRIPS', value: 'Ergonomic gel compound' },
      { label: 'SADDLE', value: 'Sport comfort design' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F900',
    price: '',
    subtitle: 'Premium grade polyethylene with exceptional optical properties and processing stability for high-quality packaging solutions.',
    specs: [
      { label: 'FRAME', value: 'Lightweight aluminum' },
      { label: 'SEAT BINDER', value: 'Quick-release system' },
      { label: 'REAR SHOCK', value: 'Coil spring suspension' },
      { label: 'FORK', value: 'Rigid carbon fork' },
      { label: 'STEM', value: 'Adjustable angle stem' },
      { label: 'HANDLEBARS', value: 'Flat bar design' },
      { label: 'GRIPS', value: 'Lock-on style' },
      { label: 'SADDLE', value: 'Racing performance' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1000',
    price: '',
    subtitle: 'Innovative polyethylene formulation with optimized melt flow for efficient production of thin gauge films.',
    specs: [
      { label: 'FRAME', value: 'Full suspension carbon' },
      { label: 'SEAT BINDER', value: 'Integrated design' },
      { label: 'REAR SHOCK', value: 'Remote lockout air shock' },
      { label: 'FORK', value: 'Triple-crown downhill fork' },
      { label: 'STEM', value: 'Direct mount' },
      { label: 'HANDLEBARS', value: 'Wide riser bar' },
      { label: 'GRIPS', value: 'Soft compound' },
      { label: 'SADDLE', value: 'Trail enduro saddle' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1100',
    price: '',
    subtitle: 'High-density compound offering excellent barrier properties and thermal stability for industrial applications.',
    specs: [
      { label: 'FRAME', value: 'Steel hardtail' },
      { label: 'SEAT BINDER', value: 'Standard clamp' },
      { label: 'REAR SHOCK', value: 'N/A - Hardtail design' },
      { label: 'FORK', value: 'Entry-level suspension' },
      { label: 'STEM', value: 'Forged aluminum' },
      { label: 'HANDLEBARS', value: 'Alloy riser' },
      { label: 'GRIPS', value: 'Basic foam' },
      { label: 'SADDLE', value: 'Comfort cruiser' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1200',
    price: '',
    subtitle: 'Versatile polyethylene grade with balanced properties for multi-purpose film applications.',
    specs: [
      { label: 'FRAME', value: 'Titanium alloy' },
      { label: 'SEAT BINDER', value: 'Titanium bolt' },
      { label: 'REAR SHOCK', value: 'Custom tuned air' },
      { label: 'FORK', value: 'Boost spacing fork' },
      { label: 'STEM', value: 'Titanium construction' },
      { label: 'HANDLEBARS', value: 'Carbon T800' },
      { label: 'GRIPS', value: 'Premium leather wrap' },
      { label: 'SADDLE', value: 'Titanium rail saddle' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1300',
    price: '',
    subtitle: 'Specialized compound for agricultural and construction film applications with superior puncture resistance.',
    specs: [
      { label: 'FRAME', value: 'Chromoly steel' },
      { label: 'SEAT BINDER', value: 'Micro-adjust' },
      { label: 'REAR SHOCK', value: 'Progressive coil' },
      { label: 'FORK', value: 'Dual crown' },
      { label: 'STEM', value: 'Low-profile design' },
      { label: 'HANDLEBARS', value: 'BMX style' },
      { label: 'GRIPS', value: 'Mushroom grip' },
      { label: 'SADDLE', value: 'Pivotal mount' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1400',
    price: '',
    subtitle: 'Next-generation polyethylene with enhanced processability and consistent output for automated production lines.',
    specs: [
      { label: 'FRAME', value: 'Composite monocoque' },
      { label: 'SEAT BINDER', value: 'Aero wedge system' },
      { label: 'REAR SHOCK', value: 'Electronic suspension' },
      { label: 'FORK', value: 'Aero road fork' },
      { label: 'STEM', value: 'Integrated cockpit' },
      { label: 'HANDLEBARS', value: 'Aero drop bar' },
      { label: 'GRIPS', value: 'Bar tape premium' },
      { label: 'SADDLE', value: 'Performance road' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1500',
    price: '',
    subtitle: 'Eco-friendly polyethylene formulation with recycled content for sustainable packaging solutions.',
    specs: [
      { label: 'FRAME', value: 'Bamboo composite' },
      { label: 'SEAT BINDER', value: 'Eco-friendly clamp' },
      { label: 'REAR SHOCK', value: 'Spring steel' },
      { label: 'FORK', value: 'Recycled carbon' },
      { label: 'STEM', value: 'Sustainable alloy' },
      { label: 'HANDLEBARS', value: 'Natural fiber bar' },
      { label: 'GRIPS', value: 'Cork grips' },
      { label: 'SADDLE', value: 'Vegan leather' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1600',
    price: '',
    subtitle: 'Ultra-thin film specialist compound with exceptional clarity and gloss for premium packaging.',
    specs: [
      { label: 'FRAME', value: 'Magnesium alloy' },
      { label: 'SEAT BINDER', value: 'Quick-flip lever' },
      { label: 'REAR SHOCK', value: 'Piggyback reservoir' },
      { label: 'FORK', value: 'Inverted design' },
      { label: 'STEM', value: 'Precision CNC' },
      { label: 'HANDLEBARS', value: 'Oversized tubing' },
      { label: 'GRIPS', value: 'Tacky rubber' },
      { label: 'SADDLE', value: 'Carbon shell' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1700',
    price: '',
    subtitle: 'Industrial-grade polyethylene with excellent chemical resistance for heavy-duty applications.',
    specs: [
      { label: 'FRAME', value: 'Reinforced steel' },
      { label: 'SEAT BINDER', value: 'Industrial clamp' },
      { label: 'REAR SHOCK', value: 'Heavy-duty coil' },
      { label: 'FORK', value: 'Rigid steel fork' },
      { label: 'STEM', value: 'Welded construction' },
      { label: 'HANDLEBARS', value: 'Reinforced bar' },
      { label: 'GRIPS', value: 'Industrial rubber' },
      { label: 'SADDLE', value: 'Durable platform' }
    ]
  }
];

const productWidth = 310;

const getProductOffset = () => {
  if (!containerRef.value) return 0;
  return (containerRef.value.offsetWidth - (containerRef.value.offsetWidth * 70) / 100) / 2;
};

const handleProductClick = async (index, event) => {
  if (activeProductIndex.value === index) return;
  
  activeProductIndex.value = index;
  
  await nextTick();
  
  const navWidth = 60;
  const coverWidth = 300;
  const left = productWidth * index + coverWidth + index * 6 + navWidth - getProductOffset();
  
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
  
  if (isMobile) {
    event.currentTarget.scrollIntoView({ inline: 'start', behavior: 'smooth' });
  } else {
    containerRef.value.scrollLeft = left;
  }
};

const closeProduct = () => {
  activeProductIndex.value = null;
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600&display=swap');

* {
  font-family: 'Montserrat', sans-serif;
}

body {
  font-family: 'Montserrat', sans-serif;
}

.detail-mode .product-active {
  opacity: 1;
}

.detail-mode > div:not(.product-active) {
  opacity: 0.3;
  transition: opacity 0.3s;
}

.detail-mode > div:not(.product-active):hover {
  opacity: 0.7;
}

@keyframes fade-up {
  from {
    opacity: 0;
    transform: translateY(100px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-up {
  animation: fade-up 0.6s both;
}

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
</style>