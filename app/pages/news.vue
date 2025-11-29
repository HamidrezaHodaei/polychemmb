<template>
  <div>
    <Navbar />
    <main class="bg-gray-100">
    <section class="previews md:flex">
      <!-- Hero Section with Background Images -->
      <div class="relative h-[30em] md:fixed md:top-0 md:left-0 md:h-screen md:w-[45%] bg-black">
        <figure
          v-for="(image, index) in backgroundImages"
          :key="index"
          class="absolute inset-0 bg-cover bg-center bg-no-repeat transition-opacity duration-1000 grayscale-img"
          :class="{ 'opacity-0': currentImageIndex !== index, 'opacity-100': currentImageIndex === index }"
          :style="{ backgroundImage: `url(${image})` }"
        />
        <div class="absolute inset-0 bg-black/20 transition-all duration-400" />
        <div class="absolute inset-0 z-10 flex flex-col items-center justify-center text-white p-8">
          <span class="uppercase tracking-wider text-sm mb-2"></span>
          <h1 class="text-4xl md:text-5xl xl:text-6xl font-bold"></h1>
        </div>
      </div>

      <!-- Content Section -->
      <div class="min-h-screen md:ml-[45%]">
        <!-- Tabs Header -->
        <header>
          <ul class="flex justify-around p-4 md:px-32 xl:px-40 uppercase tracking-wider text-sm">
            <li
              v-for="(tab, index) in tabs"
              :key="index"
              class="cursor-pointer pb-1 relative"
              :class="{ 'border-b-3 border-[#FFCD05]': activeTab === index }"
              @click="changeTab(index)"
            >
              <span
                class="relative after:content-[''] after:absolute after:bottom-0 after:left-0 after:w-0 after:border-b-3 after:border-[#FFCD05] after:transition-all after:duration-400"
                :class="{ 'hover:after:w-full': activeTab !== index }"
              >
                {{ tab }}
              </span>
            </li>
          </ul>
        </header>

        <!-- Posts Tab -->
        <div v-show="activeTab === 0" class="tab transition-opacity duration-1000" :class="{ 'opacity-100': activeTab === 0, 'opacity-0': activeTab !== 0 }">
          <ul>
            <li
              v-for="(post, index) in posts"
              :key="index"
              class="preview transition-colors duration-200"
              :class="{ 'bg-white': index % 2 === 1 }"
              @mouseenter="changeImage(index)"
            >
              <a href="#" class="block p-8 md:px-32 md:py-16 xl:px-40">
                <span class="text-black text-sm">{{ post.date }}</span>
                <h2 class="text-[#FFCD05] text-2xl md:text-3xl font-bold mb-2 mt-1">{{ post.title }}</h2>
                <p class="text-black mb-3 leading-relaxed">{{ post.excerpt }}</p>
                <span
                  class="btn-slide-down inline-block px-2 py-2 uppercase tracking-wider text-sm text-[#FFCD05] border-3 border-[#FFCD05] rounded-lg relative overflow-hidden"
                >
                  <span class="relative z-10">Read More</span>
                </span>
              </a>
            </li>
          </ul>

          <!-- Footer -->
          <footer class="flex flex-col sm:flex-row items-center justify-between px-8 py-8">
            <a href="#" class="archive-link uppercase tracking-wider text-sm relative pb-1">Archive</a>
            
          </footer>
        </div>

        <!-- Categories Tab -->
        <div v-show="activeTab === 1" class="tab transition-opacity duration-1000" :class="{ 'opacity-100': activeTab === 1, 'opacity-0': activeTab !== 1 }">
          <ul class="grid grid-cols-1 md:grid-cols-2 gap-6 md:gap-8 p-6 md:p-8 xl:p-12">
            <li v-for="(category, index) in categories" :key="index" class="card">
              <a href="#" class="block bg-white shadow-[0_1px_2px_rgba(10,10,10,0.1),0_1px_2px_rgba(10,10,10,0.2)] transition-all duration-200 group relative h-full flex flex-col">
                <div class="relative h-56 md:h-64 lg:h-72 bg-black overflow-hidden flex-shrink-0">
                  <figure
                    class="absolute inset-0 bg-cover bg-center bg-no-repeat grayscale-img"
                    :style="{ backgroundImage: `url(${category.image})` }"
                  />
                  <div class="absolute inset-0 bg-black/20 transition-all duration-400" />
                </div>
                <div class="p-6 md:p-8 flex-1 flex flex-col">
                  <h2 class="text-[#FFCD05] text-2xl md:text-3xl font-bold mb-2">{{ category.name }}</h2>
                  <p class="text-black mb-4 text-lg md:text-xl">{{ category.count }} Posts</p>
                  <span
                    class="btn-slide-down inline-block px-5 py-3 uppercase tracking-wider text-sm md:text-base text-[#FFCD05] border-2 border-[#FFCD05] rounded-lg relative overflow-hidden mt-auto"
                  >
                    <span class="relative z-10">View All</span>
                  </span>
                </div>
              </a>
            </li>
          </ul>

          <!-- Footer -->
          <footer class="flex flex-col sm:flex-row items-center justify-between px-8 py-8">
            <a href="#" class="archive-link uppercase tracking-wider text-sm relative pb-1">Archive</a>
            
          </footer>
        </div>
      </div>
    </section>
  </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Navbar from '~/components/navbar.vue'
import Footer from '~/components/footer.vue'

const tabs = ['Posts', 'Categories']
const activeTab = ref(0)
const currentImageIndex = ref(0)

const backgroundImages = [
  '/invitation-2.jpg',
  'https://images.unsplash.com/photo-1511593358241-7eea1f3c84e5?w=2000&h=1200&fit=crop',
  'https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=2000&h=1200&fit=crop',
  'https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?w=2000&h=1200&fit=crop',
  'https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=2000&h=1200&fit=crop'
]

const posts = [
  {
    date: 'Dec 3-6, 2025',
    title: 'Plast Eurasia İstanbul 2025',
    excerpt: ''
  },
  {
    date: 'Sep 7, 2016',
    title: 'Craft Beer',
    excerpt: 'Tbh vaporware mumblecore iceland echo park DIY. Plaid woke next level enamel pin, vegan cred salvia pug. XOXO sartorial synth gluten-free, cold-pressed mumblecore craft beer helvetica. Vegan lyft squid, vice...'
  },
  {
    date: 'Sep 6, 2016',
    title: 'Next Level Blog',
    excerpt: 'Humblebrag ramps single-origin coffee, literally jean shorts polaroid mlkshk franzen williamsburg distillery venmo. Skateboard leggings disrupt banjo shoreditch blue bottle. Brooklyn church-key cronut hell of waistcoat, polaroid lomo chambray bitters...'
  },
  {
    date: 'Sep 5, 2016',
    title: 'VHS Selfies',
    excerpt: '8-bit typewriter scenester, crucifix tousled tilde leggings brunch chicharrones salvia deep v man bun. Master cleanse man braid disrupt banjo, deep v cray tumblr cronut. Truffaut street art everyday carry...'
  },
  {
    date: 'Sep 4, 2016',
    title: 'Four Dollar Toast',
    excerpt: 'Flexitarian fixie keytar vice craft beer. Forage normcore cred austin brunch, put a bird on it actually. Chia put a bird on it skateboard, salvia paleo heirloom semiotics knausgaard selvage...'
  }
]

const categories = [
  {
    name: 'Brunch',
    count: 3,
    image: 'https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=500&h=300&fit=crop'
  },
  {
    name: 'Gluten-free',
    count: 2,
    image: 'https://images.unsplash.com/photo-1504674900247-0877df9cc836?w=500&h=300&fit=crop'
  },
  {
    name: 'Cities',
    count: 3,
    image: 'https://images.unsplash.com/photo-1449824913935-59a10b8d2000?w=500&h=300&fit=crop'
  },
  {
    name: 'Juice',
    count: 2,
    image: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=500&h=300&fit=crop'
  }
]

const changeTab = (index) => {
  activeTab.value = index
}

const changeImage = (index) => {
  currentImageIndex.value = index
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap');

body {
  font-family: 'Montserrat', sans-serif;
}

h1, h2 {
  font-family: 'Montserrat', sans-serif;
}

.border-b-3 {
  border-bottom-width: 3px;
}

.border-3 {
  border-width: 3px;
}

.grayscale-img {
  filter: grayscale(100%);
}

/* Hover filled slide down button effect */
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

/* Archive link hover underline effect - yellow, from left */
.archive-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 3px;
  background-color: #FFCD05;
  transition: width 0.4s ease;
}

.archive-link:hover::after {
  width: 100%;
}
</style>