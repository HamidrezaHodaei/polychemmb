<template>
  <div class="h-screen bg-[#f1f2f2] flex items-center justify-center bg-cover bg-center bg-blend-overlay" style="background-image: url('https://images.unsplash.com/photo-1598322508096-5ca34544d8aa?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80')">
    <div 
      ref="containerRef"
      :class="['w-full h-full flex overflow-x-auto overflow-y-hidden scroll-smooth bg-[#ffffff] shadow-2xl', {'detail-mode': activeProductIndex !== null}]"
    >
      <!-- Navigation Sidebar -->
      <nav class="w-[60px] lg:w-[100px] bg-gray-900 flex flex-col items-center justify-between py-7 px-4 text-white flex-shrink-0 relative z-10">
        <div class="w-8 text-red-600">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 92.91 92.91" fill="currentColor">
            <path d="M51.6 92.51v-1.63c.01-5.24.03-10.48.02-15.72 0-2.5-.94-3.53-3.47-3.52-3.63.01-7.19-.44-10.64-1.56-10.43-3.37-15.76-10.83-17.06-21.46-.19-1.57-.28-3.17-.32-4.75-.05-2.2.86-3.27 3.04-3.63.6-.1 1.21-.08 1.82-.08l33.56-.01c1.86 0 2.47-.81 1.47-2.38-2.69-4.23-6.38-7.18-11.55-7.44-4.53-.22-9.08-.04-13.62-.04-3.03 0-3.8-.75-3.83-3.81-.03-3.42-.02-6.84-.02-10.25v-1.94c-7.96.63-19.36 16.97-20.46 28.2a36.48 36.48 0 0030.58 39.65v10.4a44.47 44.47 0 01-24.01-10.1C7.3 74.38 1.53 63.99.26 51.34-2.64 22.7 19.45 2.22 41.2.36v16.55c.01 2.6.56 3 3.21 3.22 3.02.25 6.07.53 9 1.24 10.29 2.5 17.18 10.4 18.45 20.9a49 49 0 01.32 5.72c0 1.88-1.06 2.91-2.94 3.06-.7.06-1.4.03-2.1.03-10.87 0-21.73.04-32.59-.05-2.1-.02-2.85 1.22-1.68 3.06 2.53 3.94 6.13 6.38 10.77 6.77 4.18.36 8.4.23 12.61.32.8.01 1.6-.03 2.4.05 2.34.25 3.43 1.4 3.45 3.76.04 3.98.01 7.96.02 11.94 0 .53.09 1.07.15 1.8a31.76 31.76 0 008.28-5.53c6.46-5.9 10.48-13.2 11.59-21.86 1.51-11.88-2.06-22.19-10.56-30.67-5.18-5.17-11.47-8.3-18.63-9.73-.42-.08-1.08-.48-1.1-.75-.07-3.26-.04-6.52-.04-9.8 20.01 1.8 39.98 18.95 41.05 43.93a46.21 46.21 0 01-41.25 48.2z"/>
          </svg>
        </div>
        
        <div class="hidden lg:block [writing-mode:vertical-rl] rotate-180 text-sm">
          Specialized - Mountain Bikes
        </div>
        
        <div class="relative flex items-center justify-center flex-col">
          <span class="absolute -top-2 -right-1.5 bg-red-600 border-2 border-gray-900 h-5 w-5 flex items-center justify-center rounded-full text-xs font-medium text-white">3</span>
          <svg class="w-6 h-6" xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24">
            <circle cx="9" cy="21" r="1"/>
            <circle cx="20" cy="21" r="1"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6"/>
          </svg>
        </div>
      </nav>

      <!-- Cover Image -->
      <div class="w-[300px] lg:w-[500px] flex items-center justify-center text-5xl text-white font-medium text-center flex-shrink-0 bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1598322508096-5ca34544d8aa?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=934&q=80')">
        Mountain<br>Bikes
      </div>

      <!-- Products -->
      <div
        v-for="(product, index) in products"
        :key="index"
        :data-index="index"
        @click="handleProductClick(index, $event)"
        :class="[
          'flex flex-col items-center p-9 bg-[#f1f2f2] w-[310px] overflow-y-auto scroll-smooth transition-all duration-500 flex-shrink-0 ml-1.5 relative',
          activeProductIndex === index ? 'product-active w-full lg:w-[70%] px-8 lg:px-[70px] pb-0' : 'hover:shadow-[inset_0_-4px_0_0_#fc302b] cursor-pointer'
        ]"
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

        <!-- NEW Badge -->
        <div class="absolute top-9 right-9 text-xs font-medium tracking-wider">NEW</div>

        <!-- Product Image -->
        <img 
          :src="product.img" 
          :alt="product.title"
          :class="['w-full object-contain my-10 max-w-[500px] transition-transform duration-300', activeProductIndex !== index && 'hover:scale-110']"
        />

        <!-- Brand -->
        <div :class="['text-sm tracking-widest transition-all duration-500', activeProductIndex === index && 'text-base']">
          {{ product.brand }}
        </div>

        <!-- Title -->
        <h2 :class="['text-center font-medium my-5 transition-all duration-500', activeProductIndex === index ? 'text-3xl lg:text-[44px] max-w-none' : 'text-xl max-w-[12ch]']">
          {{ product.title }}
        </h2>

        <!-- Price -->
        <div :class="['mb-8 transition-all duration-500', activeProductIndex === index && 'text-2xl']">
          {{ product.price }}
        </div>

        <!-- Buttons (shown when active) -->
        <div v-if="activeProductIndex === index" class="flex items-center min-w-[80%] mb-9 animate-fade-up flex-col lg:flex-row" style="animation-delay: 0.2s">
          <div class="flex items-center">
            <button class="bg-transparent border border-gray-900 px-5 py-3 text-sm font-medium tracking-widest -mr-px rounded-l">SML</button>
            <button class="bg-gray-900 text-white border border-gray-900 px-5 py-3 text-sm font-medium tracking-widest -mr-px">MED</button>
            <button class="bg-transparent border border-gray-900 px-5 py-3 text-sm font-medium tracking-widest -mr-px">LRG</button>
            <button class="bg-transparent border border-gray-900 px-5 py-3 text-sm font-medium tracking-widest rounded-r">XLG</button>
          </div>
          <button class="bg-red-600 text-white px-5 py-3 text-sm font-medium tracking-widest flex-grow rounded ml-0 lg:ml-4 mt-4 lg:mt-0 w-full lg:w-auto">ADD TO CART</button>
        </div>

        <!-- Subtitle -->
        <p v-if="activeProductIndex !== index" class="text-gray-700 leading-relaxed text-sm mb-5">
          {{ product.subtitle }}
        </p>

        <!-- Expanded Content (shown when active) -->
        <template v-if="activeProductIndex === index">
          <p class="text-gray-700 leading-relaxed text-sm mb-5 animate-fade-up" style="animation-delay: 0.3s">
            {{ product.subtitle }}
          </p>
          
          <img 
            src="https://images.unsplash.com/photo-1555919965-a43982be9034?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=1900&q=80"
            alt="Detail"
            class="block mt-5 -mx-8 lg:-mx-[70px] max-w-none w-[calc(100%+64px)] lg:w-[calc(100%+140px)]"
          />

          <p class="text-gray-700 leading-relaxed text-sm mb-5 animate-fade-up" style="animation-delay: 0.4s">
            Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quod, quos? Nesciunt quibusdam corporis quo repellendus sequi. Molestiae repellendus ab vitae mollitia distinctio quod molestias quis, suscipit magni id ex in?
          </p>

          <p class="text-gray-700 leading-relaxed text-sm mb-5 animate-fade-up" style="animation-delay: 0.5s">
            Nostrum quibusdam, sunt deleniti vel sapiente modi tempore ea omnis non adipisci earum totam illo esse quo voluptatem dignissimos excepturi saepe! Minima vero consequatur eos laudantium deleniti architecto ducimus quia?
          </p>

          <!-- Technical Specifications Table -->
          <div class="flex flex-col bg-gray-900 text-white -mx-8 lg:-mx-[70px] px-8 lg:px-[100px] py-12 lg:py-[70px] w-[calc(100%+64px)] lg:w-[calc(100%+140px)]">
            <h3 class="font-semibold tracking-widest text-xl mb-8">TECHNICAL SPECIFICATIONS</h3>
            
            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">FRAME</div>
              <div class="leading-relaxed text-sm">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Quae optio a illo, earum, iste doloribus, quo id itaque veniam sapiente unde officia impedit maxime facere.</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">SEAT BINDER</div>
              <div class="leading-relaxed text-sm">Lorem ipsum dolor</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">REAR SHOCK</div>
              <div class="leading-relaxed text-sm">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Quae optio a illo, earum, iste doloribus.</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">FORK</div>
              <div class="leading-relaxed text-sm">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Quae optio a illo, earum, iste doloribus, quo id itaque veniam sapiente unde officia placeat non libero ducimus</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">STEM</div>
              <div class="leading-relaxed text-sm">Quae optio a illo</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">HANDLEBARS</div>
              <div class="leading-relaxed text-sm">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Quae optio a illo, earum, iste doloribus.</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">GRIPS</div>
              <div class="leading-relaxed text-sm">Iste doloribus.</div>
            </div>

            <div class="flex flex-col lg:flex-row pt-3 mt-3 border-t border-white/10">
              <div class="flex-shrink-0 font-medium tracking-widest w-full lg:w-[260px] text-white/70 mb-1 lg:mb-0">SADDLE</div>
              <div class="leading-relaxed text-sm">Minima vero consequatur eos laudantium iste doloribus. Lorem ipsum dolor sit amet consectetur.</div>
            </div>

            <button class="border border-gray-700 bg-gray-700 rounded px-5 py-3 text-sm font-medium tracking-widest text-center mt-8">ALL SPECIFICATIONS</button>
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
        <button v-else class="mt-auto font-medium text-sm tracking-widest">VIEW DETAILS</button>
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
    brand: 'TURBO LEVO',
    title: 'Turbo Levo SL Expert Carbon',
    price: '$13,200',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/Slash98XT_21_33019_A_Primary?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Id error mollitia veritatis odio repudiandae. Suscipit dolorum sunt rem aspernatur saepe sint sequi aut quis. Doloribus neque laborum fugiat officiis quae.'
  },
  {
    brand: 'ENDURO',
    title: 'Powerfly FS 9 Equipped',
    price: '$9,200',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/Slash7NX_21_32992_A_Primary?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit. Doloribus neque laborum fugiat officiis quae. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Pariatur, in delectus tempore iusto dicta odio quas a placeat earum, accusamus rem ut aut nobis, dolorem incidunt volupta.'
  },
  {
    brand: 'EPIC',
    title: 'Marlin 6 Women\'s',
    price: '$6,700',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/Marlin6Womens_20_28788_A_Primary?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit. Doloribus neque laborum fugiat officiis quae. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Pariatur, in delectus tempore iusto dicta odio quas a placeat earum, accusamus rem ut aut nobis, dolorem incidu.'
  },
  {
    brand: 'STUMPJUMPER',
    title: 'S-Worls Enduro Epic Hard',
    price: '$8,500',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/TopFuel7SX_21_32984_B_Primary?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit. Doloribus neque laborum fugiat officiis quae. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Pariatur, in delectus tempore iusto dicta odio quas a placeat.'
  },
  {
    brand: 'TURBO LEVO',
    title: 'Ticket DJ Frameset',
    price: '$12,700',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/XCaliber9_20_29760_B_Primary?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit. Doloribus neque laborum fugiat officiis quae. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Pariatur, in delectus tempore iusto dicta odio quas a placeat earum, accusamus rem ut aut nobis, dolorem incidunt volu.'
  },
  {
    brand: 'EPIC',
    title: 'Precaliber Suspension',
    price: '$15,900',
    img: 'https://trek.scene7.com/is/image/TrekBicycleProducts/1008601_2017_A_1_820_Womens?$responsive-pjpg$&cache=on,on&wid=640&hei=480',
    subtitle: 'Lorem ipsum dolor sit. Doloribus neque laborum fugiat officiis quae. Lorem ipsum dolor sit amet consectetur, adipisicing elit. Pariatur, in delectus tempore iusto dicta odio quas a placeat earum, accusamus rem ut aut'
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
  
  // Calculate scroll position
  const navWidth = 60; // Approximate nav width on mobile, 100 on desktop
  const coverWidth = 300; // Approximate cover width on mobile, 500 on desktop
  const left = productWidth * index + coverWidth + index * 6 + navWidth - getProductOffset();
  
  // Check if mobile device
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
</style>