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
            :src="product.detailImage || '/955w-p.jpg'"
            alt="Detail"
            class="block mt-5 -mx-8 lg:-mx-[70px] max-w-none w-[calc(100%+64px)] lg:w-[calc(100%+140px)]"
          />

          <!-- Technical Specifications Table -->
          <div class="flex flex-col bg-[#A8A8A8] text-white -mx-8 lg:-mx-[70px] px-8 lg:px-[100px] py-12 lg:py-[70px] w-[calc(100%+64px)] lg:w-[calc(100%+140px)]">
            <h3 class="font-semibold tracking-widest text-2xl mb-8 text-[#FFCD05]">TECHNICAL SPECIFICATIONS</h3>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 text-xs font-semibold tracking-[0.35em] uppercase text-[#FFCD05] pb-4 border-b border-white/30">
              <span>Properties</span>
              <span>Test Method</span>
              <span>Unit</span>
              <span>Typical Value</span>
            </div>

            <div
              v-for="(spec, idx) in product.specs"
              :key="idx"
              class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 py-4 border-b border-white/10 text-sm"
            >
              <div class="font-semibold tracking-wide text-white/90">{{ spec.property }}</div>
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
import { ref, nextTick } from 'vue';

const containerRef = ref(null);
const activeProductIndex = ref(null);

const products = [
  {
    
    title: 'Rotochem 0955W',
    price: '',
    subtitle: `Rotochem 0955W is a specialized white color plastic compound designed for rotational molding applications. It is a compound of linear medium density polyethylene copolymer grade with a narrow molecular weight distribution with Tio2 that possesses a range of beneficial characteristics. Rotochem is an ideal choice for manufacturers who require a reliable and durable material that can withstand harsh environmental conditions. Whether utilized in industrial or consumer applications, this plastic material is a suitable choice for those seeking consistent and stable materials.`,
    detailText: 'Rotochem 0955W delivers bright, uniform walls for large molded parts while maintaining impressive impact strength after long UV exposure. Customers rely on it for agricultural tanks, medical housings, and premium consumer bins where cosmetic consistency matters as much as durability.',
    detailImage: '/rotochem-0955w-detail.jpg',
    detailSections: [
      {
        title: 'DESCRIPTION',
        body: `Rotochem 0955W delivers bright, uniform walls for large molded parts while maintaining impressive impact strength after long UV exposure. Customers rely on it for agricultural tanks, medical housings, and premium consumer bins where cosmetic consistency matters as much as durability.`,
      },
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
    brand: 'POLYFIL',
    title: 'POLYFIL F800',
    price: '',
    subtitle: 'Advanced polyethylene compound designed for superior film processing with enhanced mechanical strength and clarity for demanding packaging applications.',
    detailImage: '/polyfil-f800-detail.jpg',
    detailText: 'POLYFIL F800 is tuned for multi-layer blown film lines that demand tight gauge control. Its clean melt and balanced rheology keep bubble stability high, so converting teams can push throughput without sacrificing clarity or seal performance.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Carbon fiber composite' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Aluminum alloy' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'High-performance air suspension' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Adjustable travel fork system' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'CNC machined aluminum' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Carbon fiber reinforced' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Ergonomic gel compound' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Sport comfort design' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F900',
    price: '',
    subtitle: 'Premium grade polyethylene with exceptional optical properties and processing stability for high-quality packaging solutions.',
    detailImage: '/polyfil-f900-detail.jpg',
    detailText: 'POLYFIL F900 focuses on shine and gloss, making luxury flexible pouches look crisp on the shelf. The resin resists haze during rapid cooling, so print colors stay vibrant and metallized layers bond evenly.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Lightweight aluminum' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Quick-release system' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Coil spring suspension' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Rigid carbon fork' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Adjustable angle stem' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Flat bar design' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Lock-on style' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Racing performance' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1000',
    price: '',
    subtitle: 'Innovative polyethylene formulation with optimized melt flow for efficient production of thin gauge films.',
    detailImage: '/polyfil-f1000-detail.jpg',
    detailText: 'Production managers deploy POLYFIL F1000 when lightweight liners need both puncture resistance and effortless sealing. The compound flows quickly at moderate temperatures, lowering energy use while keeping dart impact numbers high.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Full suspension carbon' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Integrated design' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Remote lockout air shock' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Triple-crown downhill fork' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Direct mount' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Wide riser bar' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Soft compound' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Trail enduro saddle' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1100',
    price: '',
    subtitle: 'High-density compound offering excellent barrier properties and thermal stability for industrial applications.',
    detailImage: '/polyfil-f1100-detail.jpg',
    detailText: 'POLYFIL F1100 is the go-to grade for rigid containers that face aggressive detergents or hot-fill scenarios. Its crystalline structure locks in shape under load, and machinability stays predictable for complex molds.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Steel hardtail' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Standard clamp' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'N/A - Hardtail design' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Entry-level suspension' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Forged aluminum' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Alloy riser' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Basic foam' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Comfort cruiser' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1200',
    price: '',
    subtitle: 'Versatile polyethylene grade with balanced properties for multi-purpose film applications.',
    detailImage: '/polyfil-f1200-detail.jpg',
    detailText: 'POLYFIL F1200 balances stiffness with tear strength, so converters can run one SKU across pallet stretch wraps, protective furniture covers, and agricultural mulch films with minimal recipe tweaks.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Titanium alloy' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Titanium bolt' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Custom tuned air' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Boost spacing fork' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Titanium construction' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Carbon T800' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Premium leather wrap' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Titanium rail saddle' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1300',
    price: '',
    subtitle: 'Specialized compound for agricultural and construction film applications with superior puncture resistance.',
    detailImage: '/polyfil-f1300-detail.jpg',
    detailText: 'POLYFIL F1300 shields greenhouse films from rough handling thanks to its reinforced molecular network. Even after repeated folding, the film resists pin-hole formation and maintains light diffusion.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Chromoly steel' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Micro-adjust' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Progressive coil' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Dual crown' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Low-profile design' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'BMX style' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Mushroom grip' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Pivotal mount' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1400',
    price: '',
    subtitle: 'Next-generation polyethylene with enhanced processability and consistent output for automated production lines.',
    detailImage: '/polyfil-f1400-detail.jpg',
    detailText: 'Automation engineers like POLYFIL F1400 because its melt stays steady across long production runs. Sensors pick up fewer torque spikes, meaning extruders can run faster while downstream winders keep perfect roll edges.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Composite monocoque' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Aero wedge system' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Electronic suspension' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Aero road fork' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Integrated cockpit' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Aero drop bar' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Bar tape premium' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Performance road' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1500',
    price: '',
    subtitle: 'Eco-friendly polyethylene formulation with recycled content for sustainable packaging solutions.',
    detailImage: '/polyfil-f1500-detail.jpg',
    detailText: 'POLYFIL F1500 incorporates certified recycled content without the usual odor or gel issues. It helps sustainability teams hit recycled mandates while preserving the smooth surface finish needed for brand graphics.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Bamboo composite' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Eco-friendly clamp' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Spring steel' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Recycled carbon' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Sustainable alloy' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Natural fiber bar' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Cork grips' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Vegan leather' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1600',
    price: '',
    subtitle: 'Ultra-thin film specialist compound with exceptional clarity and gloss for premium packaging.',
    detailImage: '/polyfil-f1600-detail.jpg',
    detailText: 'POLYFIL F1600 targets luxury packaging where ultra-thin gauges must drape cleanly around complex shapes. The resin resists whitening when creased, so folding cartons and overwraps look pristine in retail lighting.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Magnesium alloy' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Quick-flip lever' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Piggyback reservoir' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Inverted design' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Precision CNC' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Oversized tubing' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Tacky rubber' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Carbon shell' }
    ]
  },
  {
    brand: 'POLYFIL',
    title: 'POLYFIL F1700',
    price: '',
    subtitle: 'Industrial-grade polyethylene with excellent chemical resistance for heavy-duty applications.',
    detailImage: '/polyfil-f1700-detail.jpg',
    detailText: 'POLYFIL F1700 stands up to solvents, oils, and caustic wash-down cycles in factories. Maintenance teams favor it for liners, chute systems, and separators that need lasting slick surfaces to keep bulk materials moving.',
    specs: [
      { property: 'Frame', testMethod: '-', unit: '-', typicalValue: 'Reinforced steel' },
      { property: 'Seat Binder', testMethod: '-', unit: '-', typicalValue: 'Industrial clamp' },
      { property: 'Rear Shock', testMethod: '-', unit: '-', typicalValue: 'Heavy-duty coil' },
      { property: 'Fork', testMethod: '-', unit: '-', typicalValue: 'Rigid steel fork' },
      { property: 'Stem', testMethod: '-', unit: '-', typicalValue: 'Welded construction' },
      { property: 'Handlebars', testMethod: '-', unit: '-', typicalValue: 'Reinforced bar' },
      { property: 'Grips', testMethod: '-', unit: '-', typicalValue: 'Industrial rubber' },
      { property: 'Saddle', testMethod: '-', unit: '-', typicalValue: 'Durable platform' }
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