<script setup>
// اگر نیاز به داده دینامیک داری بعداً اینجا اضافه کن
</script>

<template>
  <div class="container">
    <!-- بخش چپ - گالری تصاویر -->
    <div class="contentLeft">
      <div class="row">
        <div class="imgWrapper">
          <img
            src="https://images.unsplash.com/photo-1687579521048-217e24217d53?crop=entropy&cs=srgb&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2ODg5ODcxNzl8&ixlib=rb-4.0.3&q=85"
            alt="Gallery image 1"
          />
        </div>
        <div class="imgWrapper">
          <img
            src="https://images.unsplash.com/photo-1686580546412-80e80519abd7?crop=entropy&cs=srgb&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2ODg5ODcyMDN8&ixlib=rb-4.0.3&q=85"
            alt="Gallery image 2"
          />
        </div>
        <div class="imgWrapper">
          <img
            src="https://images.unsplash.com/photo-1688133338995-a394ce7314e4?crop=entropy&cs=srgb&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2ODg5ODcyMDN8&ixlib=rb-4.0.3&q=85"
            alt="Gallery image 3"
          />
        </div>
        <div class="imgWrapper">
          <img
            src="https://images.unsplash.com/photo-1686354715732-7e4685269a25?crop=entropy&cs=srgb&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE2ODg5ODcyNzN8&ixlib=rb-4.0.3&q=85"
            alt="Gallery image 4"
          />
        </div>
      </div>
    </div>

    <!-- بخش راست - متن -->
    <div class="contentRight">
      <div class="content">

        <h2>About Us ...</h2>
        <p>
          POLYCHEM is a specialist producer of advanced polymer compounds and masterbatches based in the Aras Free Zone. Combining years of industrial experience with the expertise of leading engineers and researchers, we develop high-performance materials tailored for modern manufacturing needs.
        </p>
        <h4 class="text-[50px]">What We Do</h4>
        <ul>
          <li>Advanced polymer compounds &amp; masterbatches</li>
          <li>Technical know-how development &amp; transfer</li>
          <li>Engineering and design support for polymer production</li>
        </ul>
        <p>
          At POLYCHEM, we turn innovation and expertise into reliable, high-quality polymer solutions for industries that demand excellence.
        </p>
        <!-- changed: use same slide-down button style as other pages -->
        <a href="/careers" class="btn-slide-down read-more">
          <span class="relative z-10">Join Us...</span>
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* filepath: d:\Hodaei\Website\polychem\app\components\Aboutus.vue */
/* use Montserrat and match site bg + text colors */
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;400;500;600;700;800;900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: none;
}

body {
  width: 100%;
  min-height: 100vh;
  display: grid;
  place-items: center;
  font-family: 'Montserrat', sans-serif;
  background: #f1f2f2; /* match other pages */
}

.container {
  width: 90%;
  max-width: 1170px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  align-items: center;
  gap: 60px;
  padding: 35px 0;
  margin: 0 auto;
}

.contentLeft,
.contentRight {
  width: 100%;
}

.contentLeft .row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

.contentLeft .row .imgWrapper {
  width: 100%;
  height: 450px;
  overflow: hidden;
  border-radius: 10px;
  cursor: pointer;
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.15);
}

.contentLeft .row .imgWrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  user-select: none;
  transition: transform 0.3s ease, filter 0.3s ease;
  filter: grayscale(100%);
}

.contentLeft .row .imgWrapper:hover img {
  transform: scale(1.5);
}

/* جابجایی عمودی تصاویر فرد و زوج */
.contentLeft .row .imgWrapper:nth-child(odd) {
  transform: translateY(-20px);
}

.contentLeft .row .imgWrapper:nth-child(even) {
  transform: translateY(20px);
}

.contentRight .content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 15px;
}

.contentRight .content h4 {
  font-size: 30px;
  font-weight: 400;
  color: #848484;
}

.contentRight .content h2 {
  font-size: 50px;
  color: #848484; /* changed */
}

.contentRight .content p {
  font-size: 16px;
  color: #848484; /* changed */
  line-height: 28px;
  padding-bottom: 10px;
}

/* add list styles so bullets appear (reset removed default padding) */
.contentRight .content ul {
  list-style: disc;
  padding-left: 1.25rem; /* space for bullets */
  margin: 0 0 10px 0;
  color: #848484;
  line-height: 1.8;
}
.contentRight .content ul li {
  margin: 6px 0;
  /* optionally adjust marker position */
  list-style-position: outside;
}

/* Read More as site-standard yellow slide-down button */
.read-more {
  display: inline-block;
  text-decoration: none;
  font-size: 16px;
  letter-spacing: 1px;
  padding: 13px 30px;
  color: #FFCD05;
  border: 2px solid #FFCD05;
  border-radius: 8px;
  position: relative;
  overflow: hidden;
  background: transparent;
  transition: color 200ms;
}

/* reuse slide-down pseudo effect used elsewhere */
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

/* ensure inner text stays above the pseudo element */
.read-more .relative.z-10 {
  position: relative;
  z-index: 10;
}

/* ریسپانسیو */
@media (max-width: 768px) {
  .container {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .contentLeft .row {
    grid-template-columns: repeat(2, 1fr);
  }

  .contentLeft .row .imgWrapper {
    height: 150px;
  }

  .contentLeft .row .imgWrapper:nth-child(odd),
  .contentLeft .row .imgWrapper:nth-child(even) {
    transform: translateY(0);
  }

  .contentRight .content h4 {
    font-size: 18px;
  }

  .contentRight .content h2 {
    font-size: 30px;
  }
}

</style>