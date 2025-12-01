<template>
  <section class="chemistry-cards-section">
    <div class="container">
      <div class="cards-container">
        <!-- ردیف اول - 4 کارت بالا -->
        <div class="row row-top">
          <div class="col" v-for="(card, index) in cards.slice(0, 4)" :key="index">
            <div class="single-card-wrapper">
              <a :href="card.link">
                <div 
                  class="card-hover-container"
                >
                  <div 
                    class="card-image"
                    :style="{ backgroundImage: `url(${card.image})` }"
                  ></div>
                  <div class="card-content">
                    <span class="card-badge">{{ card.badge }}</span>
                    <h3 class="card-title">{{ card.title }}</h3>
                    <p class="card-date">{{ card.date }}</p>
                    <button class="card-button"><span>Read more</span></button>
                  </div>
                  <div class="card-gradient"></div>
                </div>
              </a>
            </div>
          </div>
        </div>
        
        <!-- ردیف دوم - 4 کارت پایین -->
        <div class="row row-bottom">
          <div class="col" v-for="(card, index) in cards.slice(4, 8)" :key="index + 4">
            <div class="single-card-wrapper">
              <a :href="card.link">
                <div 
                  class="card-hover-container"
                >
                  <div 
                    class="card-image"
                    :style="{ backgroundImage: `url(${card.image})` }"
                  ></div>
                  <div class="card-content">
                    <span class="card-badge">{{ card.badge }}</span>
                    <h3 class="card-title">{{ card.title }}</h3>
                    <p class="card-date">{{ card.date }}</p>
                    <button class="card-button"><span>Read more</span></button>
                  </div>
                  <div class="card-gradient"></div>
                </div>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const backgroundImages = [
  '/news1.jpg',
  '/PolypropyleneandPolypropyleneCompositesMarketzz.jpg',
  '/dynamic-data-visualization-3d.jpg',
  '/Visit-of-the-CEO-of-Bank-of-Industry.png',
  'https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=2000&h=1200&fit=crop'
]

const cards = ref([
  {
    badge: 'Event',
    title: 'Plast Eurasia İstanbul 2025',
    date: 'Dec 3-6, 2025',
    link: '/news/plast-eurasia-2025',
    image: backgroundImages[0]
  },
  {
    badge: 'Report',
    title: 'Global Polypropylene and Polypropylene Composites Market',
    date: 'Nov 30, 2025',
    link: '/news/Global-Polypropylene-and-Composites-Market-Report-2032',
    image: backgroundImages[1]
  },
  {
    badge: 'Report',
    title: 'Global Cast Low-Density Polyethylene Disposable Gloves Market',
    date: 'Dec 6, 2024',
    link: '/news/Cast_LowDensity_PE_Gloves_Market_Report_2032',
    image: backgroundImages[2]
  },
  {
    badge: 'News',
    title: 'Visit of the Head of the Central Branch of Bank of Industry',
    date: 'Nov 24, 2025',
    link: '/news/Visit-of-the-CEO-of-Bank-of-Industry',
    image: backgroundImages[3]
  },
  {
    badge: 'News',
    title: 'Four Dollar Toast',
    date: 'Sep 4, 2016',
    link: '#',
    image: backgroundImages[4]
  },
  {
    badge: 'News',
    title: 'Discover the factors influencing recycled PET',
    date: 'November 25, 2025',
    link: '#',
    image: backgroundImages[0]
  },
  {
    badge: 'Report',
    title: 'Polymer Chemistry Advances',
    date: 'October 28, 2025',
    link: '#',
    image: backgroundImages[1]
  },
  {
    badge: 'News',
    title: 'Molecular Structure Investigation',
    date: 'October 25, 2025',
    link: '#',
    image: backgroundImages[2]
  }
])
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700;800&display=swap');

*,
*::after,
*::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.chemistry-cards-section {
  min-height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f1f2f2;
  font-family: 'Montserrat', sans-serif;
  padding: 20px;
}

.container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
}

.cards-container {
  position: relative;
  width: 100%;
}

.row {
  display: flex;
  gap: 0;
  flex-wrap: nowrap;
  justify-content: center;
}

.row-top {
  margin-bottom: 0;
}

.row-bottom {
  margin-top: 0;
}

.col {
  flex: 1;
  min-width: 0;
}

.single-card-wrapper {
  height: 100%;
}

.single-card-wrapper > a {
  display: block;
  height: 100%;
  width: 100%;
  text-decoration: none;
}

.card-hover-container {
  border: 1px solid #e0e0e0;
  width: 100%;
  border-radius: 0;
  height: 100%;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  overflow: hidden;
  position: relative;
  cursor: pointer;
  min-height: 400px;
  background: #ffffff;
  transition: transform 0.3s ease;
}

.card-hover-container:hover {
  transform: translateY(-2px);
}

.card-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 1;
  transition: filter 0.5s ease, opacity 0.5s ease;
  z-index: 1;
  filter: grayscale(100%); /* make images black & white by default */
}

.card-hover-container:hover .card-image {
  filter: none; /* color on hover */
  opacity: 1; /* keep visibility; adjust if needed */
}

.card-content {
  position: relative;
  z-index: 4;
  padding: 30px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
}

.card-badge {
  display: inline-block;
  background: #555555;
  color: #ffffff;
  padding: 6px 14px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 500;
  margin-bottom: 20px;
  text-transform: capitalize;
}

.card-title {
  color: #808285;
  font-size: 24px;
  font-weight: 600;
  line-height: 1.3;
  margin-bottom: 20px;
  max-width: 90%;
}

.card-date {
  color: #666666;
  font-size: 14px;
  margin-bottom: 30px;
}

.card-button {
  background: transparent;
  color: #FFCD05;
  border: 2px solid #FFCD05;
  padding: 12px 28px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  margin-top: auto;
  position: relative;
  overflow: hidden;
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

.card-gradient {
  height: 100%;
  width: 100%;
  position: absolute;
  background: radial-gradient(circle at center, #9e9e9e 20%, #757575 40%, #616161 70%, #424242 100%);
  mix-blend-mode: overlay;
  pointer-events: none;
  z-index: 3;
  opacity: 0.5;
}

@media (max-width: 991px) {
  .row {
    flex-wrap: wrap;
  }
  
  .col {
    min-width: 50%;
    flex: 0 0 50%;
  }
  
  .card-hover-container {
    min-height: 320px;
  }

  .card-title {
    font-size: 20px;
  }

  .card-content {
    padding: 20px;
  }
}

@media (max-width: 576px) {
  .col {
    min-width: 100%;
    flex: 0 0 100%;
  }

  .card-hover-container {
    min-height: 250px;
  }

  .card-title {
    font-size: 18px;
  }
}
</style>