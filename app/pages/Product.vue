<template>
  <div class="h-screen bg-[#f1f2f2] flex items-center justify-center bg-cover bg-center bg-blend-overlay">
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
      <div class="w-[300px] lg:w-[500px] flex items-center justify-center text-5xl text-white font-medium text-center flex-shrink-0 bg-black overflow-hidden">
        <video
          ref="leftVideo"
          class="cover-video w-full h-full"
          :src="leftVideoSrc"
          :poster="leftVideoPoster"
          autoplay
          muted
          loop
          playsinline
          preload="auto"
          @error="onVideoError"
          @loadeddata="onVideoLoaded"
          aria-hidden="true"
        >
          <source :src="leftVideoSrc" :type="leftVideoType" />
          <!-- fallback image if video not supported -->
          <img src="/955w-p.jpg" alt="" />
        </video>
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

        <!-- Navigation Buttons (shown when active) -->
        <div v-if="activeProductIndex === index" class="flex items-center justify-between w-full mb-6 gap-4 animate-fade-up sticky top-0 z-10 bg-[#f1f2f2] py-2" style="animation-delay: 0.1s">
          <button
            @click.stop="previousProduct"
            :disabled="activeProductIndex === 0"
            class="flex items-center justify-center w-12 h-12 rounded-full border-2 border-[#FFCD05] text-[#FFCD05] hover:bg-[#FFCD05] hover:text-black transition-all disabled:opacity-50 disabled:cursor-not-allowed"
            aria-label="Previous product"
          >
            <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="15 18 9 12 15 6"></polyline>
            </svg>
          </button>
          
          <div class="text-sm text-gray-600 font-medium min-w-[80px] text-center">
            {{ activeProductIndex + 1 }} / {{ products.length }}
          </div>
          
          <button
            @click.stop="nextProduct"
            :disabled="activeProductIndex === products.length - 1"
            class="flex items-center justify-center w-12 h-12 rounded-full border-2 border-[#FFCD05] text-[#FFCD05] hover:bg-[#FFCD05] hover:text-black transition-all disabled:opacity-50 disabled:cursor-not-allowed"
            aria-label="Next product"
          >
            <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="9 18 15 12 9 6"></polyline>
            </svg>
          </button>
        </div>

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
          <div class="mb-5 animate-fade-up" style="animation-delay: 0.3s">
            <h4 v-if="product.subtitleTitle" class="text-sm font-semibold tracking-[0.3em] text-[#898989] uppercase mb-2">
              {{ product.subtitleTitle }}
            </h4>
            <p class="text-gray-700 leading-relaxed text-sm whitespace-pre-line">
              {{ product.subtitle }}
            </p>
          </div>
          
          <img 
            :src="product.detailImage || '/955w-p.jpg'"
            alt="Detail"
            class="block mt-5 -mx-8 lg:-mx-[70px] max-w-none w-[calc(100%+64px)] lg:w-[calc(100%+140px)]"
          />

          <!-- Technical Specifications Table -->
          <div class="flex flex-col bg-[#A8A8A8] text-white -mx-8 lg:-mx-[70px] px-8 lg:px-[100px] py-12 lg:py-[70px] w-[calc(100%+64px)] lg:w-[calc(100%+140px)]">
            <h3 class="font-semibold tracking-widest text-2xl mb-8 text-[#FFCD05]">TECHNICAL SPECIFICATIONS</h3>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 text-xs font-semibold tracking-[0.35em] uppercase text-[#FFCD05] pb-4 border-b border-white/30">
              <span>{{ product.propertyLabel || 'Properties' }}</span>
              <span>Test Method</span>
              <span>Unit</span>
              <span>Typical Value</span>
            </div>

            <div
              v-for="(spec, idx) in product.specs"
              :key="idx"
              class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 py-4 border-b border-white/10 text-sm"
            >
              <div class="font-semibold tracking-wide text-white/90">{{ spec.item || spec.property }}</div>
              <div class="text-white/80">{{ spec.testMethod }}</div>
              <div class="text-white/80">{{ spec.unit }}</div>
              <div class="text-white">{{ spec.typicalValue }}</div>
            </div>
          </div>

          <!-- Additional Narrative -->
          <div class="bg-[#f6f6f6] text-[#3d3d3d] -mx-8 lg:-mx-[70px] px-8 lg:px-[100px] py-10 w-[calc(100%+64px)] lg:w-[calc(100%+140px)] rounded-b-[20px]">
            <template v-if="product.detailSections && product.detailSections.length">
              <div
                v-for="(section, idx) in product.detailSections"
                :key="section.title || idx"
                class="space-y-2"
              >
                <h4 class="text-sm font-semibold tracking-[0.3em] text-[#898989] uppercase">
                  {{ section.title }}
                </h4>
                <p class="text-sm leading-relaxed whitespace-pre-line">
                  {{ section.body }}
                </p>
                <div class="h-6" v-if="idx !== product.detailSections.length - 1"></div>
              </div>
            </template>
            <template v-else>
              <p class="text-sm leading-relaxed whitespace-pre-line">
                {{ product.detailText }}
              </p>
            </template>
          </div>
        </template>

        <!-- View Details (shown when not active) -->
        <button v-else class="mt-auto font-medium text-sm tracking-widest text-[#FFCD05] hover:text-[#e6b800] transition-colors">VIEW DETAILS →</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, onMounted, watch } from 'vue';

const route = useRoute();
const containerRef = ref(null);
const activeProductIndex = ref(null);
const leftVideo = ref(null);

// encode path (handles spaces)
const leftVideoSrc = encodeURI('/Left side.mov');
const leftVideoPoster = '/955w-p.jpg';
const leftVideoType = leftVideoSrc.endsWith('.mp4')
  ? 'video/mp4'
  : leftVideoSrc.endsWith('.webm')
    ? 'video/webm'
    : 'video/quicktime';

const products = [
  {
    
    title: 'ROTOCHEM 0955W',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `Rotochem 0955W is a specialized white color plastic compound designed for rotational molding applications. It is a compound of linear medium density polyethylene copolymer grade with a narrow molecular weight distribution with Tio2 that possesses a range of beneficial characteristics. Rotochem is an ideal choice for manufacturers who require a reliable and durable material that can withstand harsh environmental conditions. Whether utilized in industrial or consumer applications, this plastic material is a suitable choice for those seeking consistent and stable materials.`,
    detailText: 'Rotochem 0955W delivers bright, uniform walls for large molded parts while maintaining impressive impact strength after long UV exposure. Customers rely on it for agricultural tanks, medical housings, and premium consumer bins where cosmetic consistency matters as much as durability.',
    detailImage: '/955w-p.jpg',
    detailSections: [
   
      {
        title: 'ADVANTAGE',
        body: `Rotochem exhibit a number of advantageous features, including excellent dispersion, exceptional impact strength, superior internal and external surface finish, optimal whiteness, reliable resistance to stress cracking, and efficient UV stabilization.`,
      },
      {
        title: 'PACKAGING',
        body: `Rotochem is supplied in powder form packed in 20 kg bags.`,
      },
      {
        title: 'STORAGE AND HANDLING',
        body: `Rotochem should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process Rotochem within 18 months after delivery.`,
      },
    ],
  specs: [
      { property: 'MFI (190°C / 2.16 kg)', testMethod: 'ASTM D1238-7', unit: 'g/10min', typicalValue: '3.5 ± 0.5' },
      { property: 'Density', testMethod: 'ASTM D1505-68', unit: 'kg/m³', typicalValue: '0.937 – 0.945' },
      { property: 'Titanium crystal type', testMethod: '-', unit: '-', typicalValue: 'Rutile type' },
      { property: 'TiO₂ content', testMethod: '-', unit: '%', typicalValue: '2' },
      { property: 'Moisture content', testMethod: '-', unit: 'ppm', typicalValue: '≤ 1500' },
      { property: 'Vicat softening point', testMethod: 'ASTM D1525', unit: '°C', typicalValue: '115' },
      { property: 'Tensile strength @ yield', testMethod: 'ASTM D638 - 72', unit: 'MPa', typicalValue: '15' },
      { property: 'Elongation @ break', testMethod: 'ASTM D638', unit: '%', typicalValue: '> 700' },
      { property: 'Flexural modulus', testMethod: 'ASTM D790', unit: 'MPa', typicalValue: '650' },
      { property: 'Hardness', testMethod: 'ASTM D2240', unit: 'Shore D', typicalValue: '65' },
      { property: 'ESCR (IGEPAL 10%) (F50, 50°C)', testMethod: 'ASTM D1693', unit: 'HR', typicalValue: '200' }
    ]
  },
 {
    
    title: 'ROTOCHEM 0955B',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `Rotochem 0955B is a specialized blue color plastic compound designed for rotational molding applications. It is a compound of linear medium density polyethylene copolymer grade with a narrow molecular weight distribution with Tio2 that possesses a range of beneficial characteristics. Rotochem is an ideal choice for manufacturers who require a reliable and durable material that can withstand harsh environmental conditions. Whether utilized in industrial or consumer applications, this plastic material is a suitable choice for those seeking consistent and stable materials.`,
    detailText: 'Rotochem 0955W delivers bright, uniform walls for large molded parts while maintaining impressive impact strength after long UV exposure. Customers rely on it for agricultural tanks, medical housings, and premium consumer bins where cosmetic consistency matters as much as durability.',
    detailImage: '/955B.jpg',
    detailSections: [
      
      {
        title: 'ADVANTAGE',
        body: `Rotochem exhibit a number of advantageous features, including excellent dispersion, exceptional impact strength, superior internal and external surface finish, optimal whiteness, reliable resistance to stress cracking, and efficient UV stabilization.`,
      },
      {
        title: 'PACKAGING',
        body: `Rotochem is supplied in powder form packed in 20 kg bags.`,
      },
      {
        title: 'STORAGE AND HANDLING',
        body: `Rotochem should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process Rotochem within 18 months after delivery.`,
      },
    ],
 specs: [
  { property: 'MFI (190°C / 5 kg)',      testMethod: 'ISO 1133',      unit: 'g/10min', typicalValue: '0.19 ± 0.1' },
  { property: 'Density',                testMethod: 'ISO 1183',      unit: 'kg/m³',   typicalValue: '0.952 ± 0.003' },
  { property: 'Moisture content',       testMethod: '-',             unit: 'ppm',     typicalValue: '≤ 1500' },
  { property: 'Melting point',          testMethod: 'ASTM D2117',    unit: '°C',      typicalValue: '135 ± 5' },
  { property: 'Vicat softening point',  testMethod: 'ASTM D1525',    unit: '°C',      typicalValue: '124' },
  { property: 'Tensile strength @ yield (MD)',       testMethod: 'ASTM D638', unit: 'MPa', typicalValue: '24' },
  { property: 'Tensile strength @ yield (MD, TD)',   testMethod: 'ASTM D638', unit: 'MPa', typicalValue: '30, 60' },
  { property: 'Elongation @ break (MD, TD)',         testMethod: 'ASTM D638', unit: '%',   typicalValue: '450, 240' },
  { property: 'ESCR (IGEPAL 10%) (F50, 50°C)',       testMethod: 'ASTM D1693', unit: 'HR',  typicalValue: '> 1000' }
]
  },
  {
 title: 'POLYFIL F700',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `POLYFIL F700 is a high-performance polyethylene compound specifically formulated for high-density polyethylene (HDPE) blown film applications. This grade is engineered to deliver superior mechanical properties, excellent film uniformity, and reliable processability, even in ultra-thin film applications.
It is highly recommended for producing films with thicknesses in the range of 10–25 microns, making it suitable for a wide range of packaging and consumer products such as shopping bags, T-shirt bags, garbage bags, liner bags, and food-contact films.`,
    detailImage: '/polyfilf700-2.jpg',
    detailSections: [
      
      {
        title: 'ADVANTAGE',
        body: `• High tensile strength with excellent dart impact resistance
• Low gel content for smooth, defect-free films
• Good moisture barrier and enhanced ultra-thin film capability
• Food contact compliant — suitable for hygienic and food packaging uses
• High stiffness ensuring good dimensional stability
• Wide service temperature range and UV resistance for outdoor durability
• Good impact resistance and excellent processability on standard HDPE film lines`,
      },
      {
        title: 'Processing Guidelines',
        body: `Processing parameters may vary depending on machine configuration, die size, and target film thickness. The following conditions are recommended as a starting point:
• Melt Temperature: 190–210 °C
• Blow-Up Ratio (BUR): 3–5
• Frost Line Height (FLH): 8–10 × die diameter`,
      },
       {
        title: 'PACKAGING',
        body: `POLYFIL F700 is supplied in powder form packed in 25 kg bags.`,
      },
      {
        title: 'STORAGE AND HANDLING',
        body: `POLYFIL F700 should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process POLYFIL F700 within 12 months after delivery.`,
      },
    ],
  specs: [
      { property: 'MFI (190°C / 2.16 kg)', testMethod: 'ASTM D1238-7', unit: 'g/10min', typicalValue: '3.5 ± 0.5' },
      { property: 'Density', testMethod: 'ASTM D1505-68', unit: 'kg/m³', typicalValue: '0.937 – 0.945' },
      { property: 'Titanium crystal type', testMethod: '-', unit: '-', typicalValue: 'Rutile type' },
      { property: 'TiO₂ content', testMethod: '-', unit: '%', typicalValue: '2' },
      { property: 'Moisture content', testMethod: '-', unit: 'ppm', typicalValue: '≤ 1500' },
      { property: 'Vicat softening point', testMethod: 'ASTM D1525', unit: '°C', typicalValue: '115' },
      { property: 'Tensile strength @ yield', testMethod: 'ASTM D638 - 72', unit: 'MPa', typicalValue: '15' },
      { property: 'Elongation @ break', testMethod: 'ASTM D638', unit: '%', typicalValue: '> 700' },
      { property: 'Flexural modulus', testMethod: 'ASTM D790', unit: 'MPa', typicalValue: '650' },
      { property: 'Hardness', testMethod: 'ASTM D2240', unit: 'Shore D', typicalValue: '65' },
      { property: 'ESCR (IGEPAL 10%) (F50, 50°C)', testMethod: 'ASTM D1693', unit: 'HR', typicalValue: '200' }
    ]
  },
    {
 title: 'HDCHEM 4760',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `HDCHEM 4760 is a specialized plastic compound designed for blow molding applications. It is a compound of polyethylene copolymers that possesses a range of beneficial characteristics. HDCHEM 4760 is an ideal choice for manufacturers who require a reliable and durable product, this plastic material is a dependable choice for those seeking consistent and stable materials.`,
    detailImage: '',
    detailSections: [
      
      {
        title: 'ADVANTAGE',
        body: `HDCHEM 4760 exhibit a number of advantageous features, including good flowability, impact strength, ESCR and rigidity.`,
      },
     
       {
        title: 'PACKAGING',
        body: `HDCHEM 4760 is supplied in powder form packed in 25 kg bags.`,
      },
      {
        title: 'STORAGE AND HANDLING',
        body: `HDCHEM 4760 should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process HDCHEM 4760 within 12 months after delivery.`,
      },
    ],
specs: [
  { property: 'MFI (190°C / 5 kg)',     testMethod: 'ISO 1133',     unit: 'g/10min',   typicalValue: '1.2 ± 0.5' },
  { property: 'Density',                testMethod: 'ISO 1183',     unit: 'kg/m³',      typicalValue: '0.954 ± 0.002' },
  { property: 'Moisture content',       testMethod: '-',            unit: 'ppm',       typicalValue: '≤ 1500' },
  { property: 'Vicat softening point',  testMethod: 'ASTM D1525',   unit: '°C',        typicalValue: '115' },
  { property: 'Impact strength (23°C)', testMethod: 'ISO 179/1eA',  unit: 'mJ/mm²',    typicalValue: '9' },
  { property: 'Hardness',               testMethod: 'ASTM D2240',   unit: 'Shore D',   typicalValue: '62' },
  { property: 'Swell ratio',            testMethod: '-',            unit: '%',         typicalValue: '120' }
]
  },
 {
 title: 'SlIPCHEM E 178',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `SlIPCHEM-E 178 is a high-performance slip masterbatch containing high quality slip agent dispersed in a polyethylene carrier resin. It is specifically developed to reduce the coefficient of friction (COF) between polymer film layers during winding, bag-making, and packaging processes. The product offers excellent dispersion, high thermal stability, and consistent migration performance.`,
    detailImage: '/slipchem.jpg',
    detailSections: [
      
      {
        title: 'Typical Applications',
        body: `• Blown film (LDPE, LLDPE, HDPE)
• Cast film (CPP, PE-based multilayers)
• Laminated and printed packaging films
• Agricultural films and shrink/stretch films`,
      },
        {
        title: 'ADVANTAGE',
        body: `• Rapid and uniform slip activation
• Consistent COF reduction
• Excellent dispersion and film clarity retention
• No interference with sealing or printing inks
• Compatible with most pigments and additives`,
      },
      {
        title: 'TYPICAL ADDITION RATE',
        body: `• Rapid and uniform slip activation
• Consistent COF reduction
• Excellent dispersion and film clarity retention
• No interference with sealing or printing inks
• Compatible with most pigments and additives`,
      },
       {
        title: 'PACKAGING',
        body: `SlIPCHEM-E 178 is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'STORAGE AND HANDLING',
        body: `SlIPCHEM-E 178 should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 40°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to pre-dry before use and process within 12 months under recommended conditions.`,
      },
    ],
specs: [
  { property: 'Appearance',                    testMethod: 'Visual',       unit: '-',                  typicalValue: 'Milky-white granular pellets' },
  { property: 'Active Content',                testMethod: '-',            unit: 'wt.%',               typicalValue: '28 ± 1' },
  { property: 'Carrier Resin',                 testMethod: '-',            unit: '–',                  typicalValue: 'LDPE / LLDPE compatible' },
  { property: 'Melt Flow Index (190°C / 2.16 kg)', testMethod: 'ASTM D1238', unit: 'g/10 min',         typicalValue: '15 ± 1' },
  { property: 'Density',                       testMethod: 'ASTM D792',    unit: 'g/cm³',              typicalValue: '0.94 – 0.96' },
  { property: 'Moisture Content',              testMethod: 'ASTM D6980',   unit: 'wt.%',               typicalValue: '< 0.2' }
]
  },
   {
 title: 'RAFCOLOR 1560',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `RAFCOLOR is the white masterbatch that consists of a high proportion of rutile titanium dioxide and thermoplastic polypropylene resin. The selected titanium dioxide has good opacity and dispersion performance. Highly-concentrated white MB with excellent dispersion and thermal stability can be applied to general-purpose products. It is recommended for raffia, Tapes, CF/BCF yarn, and other products.`,
    detailImage: '/Rafcolor-1.jpg',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `RAFCOLOR is Featured with excellent dispersion, high compatibility, good processability, low water uptake, and stable properties.`,
      },
        {
        title: 'Packaging',
        body: `white masterbatch RAFCOLOR is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `RAFCOLOR should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process PP resin within 18 months after delivery.RAFCOLOR should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process PP resin within 18 months after delivery.`,
      },
   
    ],
specs: [
  { item: 'Titanium crystal type',   testMethod: '-',           unit: '-',         typicalValue: 'Rutile type' },
  { item: 'Solid content',           testMethod: '-',           unit: '%',         typicalValue: '60 ± 2' },
  { item: 'Moisture content',        testMethod: 'ASTM D644',   unit: '%',         typicalValue: '> 0.2' },
  { item: 'Melt Flow Index (230°C / 2.16 kg)', testMethod: 'ASTM D1238', unit: 'g/10min', typicalValue: '13 ± 2' },
  { item: 'Density',                 testMethod: 'ASTM D1505',  unit: 'g/cm³',     typicalValue: '1.8' }
]
  },
{
 title: 'CALCICHEM 126 FP',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `CALCICHEM 126 FP is a polypropylene-based filler masterbatch containing 80% calcium carbonate (CaCO₃) . The CALCICHEM 126 FP features a high, very fine, treated CaCO₃ content, ensuring excellent dispersion within the final product. It is specifically designed for direct addition during the processing of polyolefins, including extrusion and injection molding. The CALCICHEM 126 FP is manufactured using the premium additives and advanced production lines.`,
    detailImage: '/public/126.mov',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `▪ Increase of productivity
▪ Low water carrying
▪ Thermal conductivity improvement
▪ Increase of the anti-blocking effect
▪ Print improvement on the end product
▪ Stable performance of the product in the production process
▪ Ensuring high loading capability for raffia production
▪ Therelatively best price-to-performance ratio`,
      },
        {
        title: 'Packaging',
        body: `CALCICHEM 126 FP is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `RAFCOLOR should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process PP resin within 18 months after delivery.RAFCOLOR should be stored to prevent direct sunlight and/or heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product perfoCALCICHEM 126 FP should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to pre-dry before use and process within 18 months after production date.rmance. It is advisable to process PP resin within 18 months after delivery.`,
      },
   
    ],
specs: [
  { item: 'Carrier',                     testMethod: '-',           unit: '-',         typicalValue: 'Polypropylene' },
  { item: 'CaCO₃ content',               testMethod: 'ASTM D4218',  unit: '%',         typicalValue: '80 ± 2' },
  { item: 'Moisture content',            testMethod: 'ASTM D644',   unit: 'ppm',       typicalValue: '≤ 1500' },
  { item: 'Melt Index (230°C / 2.16 kg)', testMethod: 'ASTM D1238', unit: 'g/10min',   typicalValue: '2 – 8' },
  { item: 'Density @ 23°C',              testMethod: 'ASTM D1505',  unit: 'g/cm³',     typicalValue: '1.css1.85 ± 0.05' }
]
  },
  {
 title: 'CALCICHEM 110 FRF',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `CALCICHEM 110 FRF is a mineral modifier with a high, very fine, treated CaCO3 content that has an excellent dispersion in the final product, indicated for direct addition in the processing of polyolefins.
The CALCICHEM 110 FRF mineral modifier is designed for films, Raffia and ropes and also suitable for general-purpose products with PE and PP carriers.`,
    detailImage: '',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `
▪ Increase of productivity
▪ Raw material costs reduction
▪ Thermal conductivity improvement
▪ Power consumption Reduction
▪ Print improvement on the end product
▪ Increase of the anti-blocking effect`,
      },
        {
        title: 'Packaging',
        body: `CALCICHEM 110 FRF is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `CALCICHEM 110 FRF should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to pre-dry before use and process within 18 months after production date.`,
      },
   
    ],
specs: [
  { property: 'Carrier',                     testMethod: '-', unit: '-',         typicalValue: 'Polypropylene' },
  { property: 'CaCO₃ content',               testMethod: '-', unit: '%',        typicalValue: '80 ± 2' },
  { property: 'Moisture content',            testMethod: '-', unit: 'ppm',      typicalValue: '≤ 1500' },
  { property: 'Melt Index (230°C / 5 kg)',   testMethod: '-', unit: 'g/10min',  typicalValue: '12 ± 2' },
  { property: 'Density @ 23°C',              testMethod: '-', unit: 'g/cm³',    typicalValue: '1.86 ± 0.05' }
]
  },{
 title: 'CALCICHEM 275 PM',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `CALCICHEM 275 PM is a polypropylene-based mineral masterbatch containing 75% ultra-fine mineral filler. It offers excellent dispersion and high mineral loading for enhanced performance in final products.
Specifically with formulated for direct addition during the extrusion of BOPP, CPP, and OPP films, CALCICHEM 275 PM delivers consistent performance and superior quality this product Manufactured with premium additives and produced on advanced production lines, it meets the highest industry standards for reliability and efficiency.`,
    detailImage: '/275.jpg',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `
▪ Improved product consistency and quality
▪ Enhanced processing stability
▪ Suitable for high-performance film applications
`,
      },
        {
        title: 'Packaging',
        body: `CALCICHEM 275 PM is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `CALCICHEM 275 PM should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 35°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to pre-dry before use and process within 18 months after production date.`,
      },
   
    ],
specs: [
  { 
  property: 'Carrier', 
  testMethod: '-', 
  unit: '-', 
  typicalValue: 'Polypropylene' 
},
{ 
  property: 'Mineral content', 
  testMethod: '-', 
  unit: '%', 
  typicalValue: '75 ± 1' 
},
{ 
  property: 'Mean particle size (d50%)', 
  testMethod: '-', 
  unit: 'μm', 
  typicalValue: '≤ 2' 
},
{ 
  property: 'Moisture content', 
  testMethod: '-', 
  unit: 'ppm', 
  typicalValue: '≤ 1500' 
},
{ 
  property: 'Melt Index (230°C / 2.16 kg)', 
  testMethod: '-', 
  unit: 'g/10min', 
  typicalValue: '3 ± 1' 
},
{ 
  property: 'Density @ 23°C', 
  testMethod: '-', 
  unit: 'g/cm³', 
  typicalValue: '1.75 ± 0.05' 
}
]
  },
  {
 title: 'CALCICHEM 110 FRF',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `CALCICHEM 110 FRF is a mineral modifier with a high, very fine, treated CaCO3 content that has an excellent dispersion in the final product, indicated for direct addition in the processing of polyolefins.
The CALCICHEM 110 FRF mineral modifier is designed for films, Raffia and ropes and also suitable for general-purpose products with PE and PP carriers.`,
    detailImage: '',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `
▪ Increase of productivity
▪ Raw material costs reduction
▪ Thermal conductivity improvement
▪ Power consumption Reduction
▪ Print improvement on the end product
▪ Increase of the anti-blocking effect`,
      },
        {
        title: 'Packaging',
        body: `CALCICHEM 110 FRF is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `CALCICHEM 110 FRF should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to pre-dry before use and process within 18 months after production date.`,
      },
   
    ],
specs: [
  { property: 'Carrier',                     testMethod: '-', unit: '-',         typicalValue: 'Polypropylene' },
  { property: 'CaCO₃ content',               testMethod: '-', unit: '%',        typicalValue: '80 ± 2' },
  { property: 'Moisture content',            testMethod: '-', unit: 'ppm',      typicalValue: '≤ 1500' },
  { property: 'Melt Index (230°C / 5 kg)',   testMethod: '-', unit: 'g/10min',  typicalValue: '12 ± 2' },
  { property: 'Density @ 23°C',              testMethod: '-', unit: 'g/cm³',    typicalValue: '1.86 ± 0.05' }
]
  },{
 title: 'CALCICHEM 120 FE',
    price: '',
    subtitleTitle: 'DESCRIPTION',
    subtitle: `CALCICHEM 120 FE is a polyolefins-based filler masterbatch containing 80% calcium carbonate (CaCO₃). The CALCICHEM 120 FE features a high, very fine, treated CaCO₃ content, ensuring excellent dispersion within the final product.
It is specifically designed for direct addition during the processing of polyolefins, including extrusion and injection molding.`,
    detailImage: '',
    propertyLabel: 'Item',
    detailSections: [
      
      {
        title: 'Advantage',
        body: `
▪ Increase of productivity
▪ Raw material costs reduction
▪ Thermal conductivity improvement
▪ Power consumption Reduction
▪ Print improvement on the end product
▪ Increase of the anti-blocking effect
`,
      },
        {
        title: 'Packaging',
        body: `CALCICHEM 120 FE is supplied in standard pellet form packed in 25 kg bags.`,
      },
      {
        title: 'Storage and handling',
        body: `CALCICHEM 120 FE should be stored to prevent direct sunlight and heat exposure. The storage area should also be dry and preferably not exceed 50°C; Bad storage conditions may lead to quality deterioration and product performance. It is advisable to process within 18 months after.`,
      },
   
    ],
specs: [
  { property: 'CaCO₃ content',              testMethod: '-', unit: '%',       typicalValue: '80 ± 1' },
  { property: 'Moisture content',           testMethod: '-', unit: 'ppm',      typicalValue: '≤ 1500' },
  { property: 'Melt Index (190°C / 5 kg)',  testMethod: '-', unit: 'g/10min',  typicalValue: '2 ± 1' },
  { property: 'Density @ 23°C',             testMethod: '-', unit: 'g/cm³',    typicalValue: '1.85 ± 0.05' }
]
  }
];

const productWidth = 310;

const getProductOffset = () => {
  if (!containerRef.value) return 0;
  const containerWidth = containerRef.value.offsetWidth;
  const visibleWidth = (containerWidth * 70) / 100;
  return (containerWidth - visibleWidth) / 2;
};

const handleProductClick = async (index, event) => {
  if (activeProductIndex.value === index) return;
  
  activeProductIndex.value = index;
  
  await nextTick();
  
  const navWidth = 60;
  const coverWidth = 300;
  const gap = 6;
  const left = index * (productWidth + gap) + coverWidth + navWidth - getProductOffset();
  
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
  
  if (isMobile) {
    event.currentTarget.scrollIntoView({ inline: 'center', behavior: 'smooth' });
  } else {
    containerRef.value.scrollLeft = left;
  }
};

const closeProduct = () => {
  activeProductIndex.value = null;
};

const scrollToProduct = async (index) => {
  if (index === null || index === undefined || index < 0 || index >= products.length) {
    return;
  }
  
  activeProductIndex.value = index;
  
  await nextTick();
  
  const navWidth = 60;
  const coverWidth = 300;
  const gap = 6;
  const left = index * (productWidth + gap) + coverWidth + navWidth - getProductOffset();
  
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
  
  if (isMobile) {
    const productElement = containerRef.value?.querySelector(`[data-index="${index}"]`);
    if (productElement) {
      productElement.scrollIntoView({ inline: 'center', behavior: 'smooth' });
    }
  } else {
    if (containerRef.value) {
      containerRef.value.scrollLeft = left;
    }
  }
};

// Watch for route query changes
watch(() => route.query.index, async (newIndex) => {
  if (newIndex !== undefined) {
    const index = parseInt(newIndex);
    if (!isNaN(index)) {
      await nextTick();
      scrollToProduct(index);
    }
  }
}, { immediate: true });

// Handle initial load
onMounted(async () => {
  await nextTick();
  const indexParam = route.query.index;
  if (indexParam !== undefined) {
    const index = parseInt(indexParam);
    if (!isNaN(index)) {
      scrollToProduct(index);
    }
  }
});

const previousProduct = async () => {
  if (activeProductIndex.value > 0) {
    const newIndex = activeProductIndex.value - 1;
    await navigateToProduct(newIndex);
  }
};

const nextProduct = async () => {
  if (activeProductIndex.value < products.length - 1) {
    const newIndex = activeProductIndex.value + 1;
    await navigateToProduct(newIndex);
  }
};

const navigateToProduct = async (index) => {
  const router = useRouter();
  await router.replace({ query: { index: index.toString() } });
  await scrollToProduct(index);
};

// Video handling
const onVideoLoaded = (e) => {
  // try to play (some browsers require a user gesture, but attempt anyway)
  const v = leftVideo.value;
  if (!v) return;
  const p = v.play();
  if (p && typeof p.then === 'function') {
    p.catch(err => {
      // silent failure; poster will remain visible
      // console.warn('Video play prevented:', err);
    });
  }
};

const onVideoError = (e) => {
  // fallback behavior: leave poster visible; developer can check console
  // console.error('Left video failed to load', e);
};

onMounted(() => {
  // attempt immediate play if loaded earlier
  nextTick().then(() => {
    if (leftVideo.value && leftVideo.value.readyState >= 2) {
      onVideoLoaded();
    }
  });
});
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

/* ensure the left cover video fills its container and keeps aspect via cover */
.cover-video {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  pointer-events: none; /* avoid intercepting clicks */
}
</style>