<template>
  <div class="min-h-screen bg-[#f1f2f2] py-16 px-4 relative">
    <!-- Globe + Contact us overlay -->
    <div class="relative flex justify-center items-center w-full" style="height: 480px; min-height: 340px;">
      <worldc
        class="absolute left-1/2 top-0 -translate-x-1/2 z-0"
        style="width: 600px; height: 480px; max-width: 100vw;"
        :scroll-rotate="scrollRotate"
      />
      <div
        class="absolute left-1/2 top-1/2 z-10"
        style="transform: translate(-50%, -50%); pointer-events: none;"
      >
        <h1 class="text-6xl font-bold text-[#848484] mb-0 text-center select-none" style="text-shadow: 0 2px 16px #fff, 0 1px 0 #fff;">
          Contact us
        </h1>
      </div>
    </div>
    <!-- Header & Tabs -->
    <div class="text-center mb-16 mt-[-120px] relative z-20">
      <!-- Navigation Tabs -->
      <div class="grid grid-cols-5 w-7xl mx-auto bg-white overflow-hidden shadow-sm">
        <button
          v-for="(tab, idx) in tabs"
          :key="tab.key"
          :class="[
            'text-[#848484] transition-colors px-8 py-6',
            idx !== tabs.length - 1 ? 'border-r border-gray-200' : '',
            activeTab === tab.key ? 'bg-gray-100 font-bold' : 'hover:bg-gray-50'
          ]"
          @click="scrollToSection(tab.key)"
        >
          {{ tab.label }}
        </button>
      </div>
    </div>

    <!-- Middle East Section -->
    <div
      id="middleEast-section"
      ref="middleEastSection"
      class="max-w-7xl mx-auto mb-12"
    >
      <h2 class="text-4xl font-bold text-[#848484] mb-8 ">Middle East</h2>
      <div class="flex overflow-x-auto gap-0 no-scrollbar">
        <div
          v-for="(country, idx) in middleEastCountries"
          :key="'me-'+idx"
          class="contact-card flex flex-col items-center justify-center text-center"
          @mousemove="handleContactMouseMove($event, countryRefsOffset + idx)"
          @mouseleave="handleContactMouseLeave(countryRefsOffset + idx)"
          @touchmove="handleContactTouchMove($event, countryRefsOffset + idx)"
          @touchend="handleContactMouseLeave(countryRefsOffset + idx)"
        >
          <h3 class="text-2xl font-semibold text-[#848484] mb-4">{{ country.name }}</h3>
          <p class="text-[#848484] mb-6">{{ country.city }}</p>
          <div class="space-y-2 w-full flex flex-col items-center">
            <p class="text-sm text-[#848484]">Email</p>
            <div class="flex items-center justify-center gap-2">
              <a :href="`mailto:${country.email}`" class="text-[#848484] hover:underline">
                {{ country.email }}
              </a>
              <button
                type="button"
                class="ml-1 px-2 py-1 rounded bg-gray-100 hover:bg-gray-200 text-xs text-[#848484] border border-gray-300 transition"
                @click="copyEmail(country.email, countryRefsOffset + idx)"
              >
                <span v-if="copiedIdx === countryRefsOffset + idx && copied">Copied!</span>
                <span v-else>Copy</span>
              </button>
            </div>
          </div>
          <!-- پس‌زمینه پرچم برای ایران، عراق و کویت با افکت موجی/مورب -->
          <div
            v-if="country.name === 'Iran'"
            class="iran-flag-bg pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[countryRefsOffset + idx] = el }"
          ></div>
          <div
            v-else-if="country.name === 'Iraq'"
            class="iraq-flag-bg iraq-diagonal-wipe pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[countryRefsOffset + idx] = el }"
          ></div>
          <div
            v-else-if="country.name === 'Kuwait'"
            class="iran-flag-bg pointer-events-none"
            :style="'background-image: url(https://upload.wikimedia.org/wikipedia/commons/a/aa/Flag_of_Kuwait.svg)'"
            :ref="el => { if (el) contactCharactersRefs[countryRefsOffset + idx] = el }"
          ></div>
          <div
            v-else
            class="contact-card-bg-characters pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[countryRefsOffset + idx] = el }"
          >
            {{ contactRandomTexts[countryRefsOffset + idx] }}
          </div>
        </div>
      </div>
    </div>

    <!-- CIS & Caucasus Section -->
    <div
      id="cis-section"
      ref="cisSection"
      class="max-w-7xl mx-auto mb-12"
    >
      <h2 class="text-4xl font-bold text-[#848484] mb-8 ">CIS & Caucasus</h2>
      <div class="flex overflow-x-auto gap-0 no-scrollbar">
        <div
          v-for="(country, idx) in cisCountries"
          :key="'cis-'+idx"
          class="contact-card flex flex-col items-center justify-center text-center"
          @mousemove="handleContactMouseMove($event, cisRefsOffset + idx)"
          @mouseleave="handleContactMouseLeave(cisRefsOffset + idx)"
          @touchmove="handleContactTouchMove($event, cisRefsOffset + idx)"
          @touchend="handleContactMouseLeave(cisRefsOffset + idx)"
        >
          <h3 class="text-2xl font-semibold text-[#848484] mb-4">{{ country.name }}</h3>
          <p class="text-[#848484] mb-6">{{ country.city }}</p>
          <div class="space-y-2 w-full flex flex-col items-center">
            <p class="text-sm text-[#848484]">Email</p>
            <div class="flex items-center justify-center gap-2">
              <a :href="`mailto:${country.email}`" class="text-[#848484] hover:underline">
                {{ country.email }}
              </a>
              <button
                type="button"
                class="ml-1 px-2 py-1 rounded bg-gray-100 hover:bg-gray-200 text-xs text-[#848484] border border-gray-300 transition"
                @click="copyEmail(country.email, cisRefsOffset + idx)"
              >
                <span v-if="copiedIdx === cisRefsOffset + idx && copied">Copied!</span>
                <span v-else>Copy</span>
              </button>
            </div>
          </div>
          <div
            class="contact-card-bg-characters pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[cisRefsOffset + idx] = el }"
          >
            {{ contactRandomTexts[cisRefsOffset + idx] }}
          </div>
        </div>
      </div>
    </div>

    <!-- Europe Section -->
    <div
      id="europe-section"
      ref="europeSection"
      class="max-w-7xl mx-auto mb-12"
    >
      <h2 class="text-4xl font-bold text-[#848484] mb-8 text">Europe</h2>
      <div class="flex overflow-x-auto gap-0 no-scrollbar">
        <div
          v-for="(country, idx) in europeCountries"
          :key="'eu-'+idx"
          class="contact-card flex flex-col items-center justify-center text-center"
          @mousemove="handleContactMouseMove($event, europeRefsOffset + idx)"
          @mouseleave="handleContactMouseLeave(europeRefsOffset + idx)"
          @touchmove="handleContactTouchMove($event, europeRefsOffset + idx)"
          @touchend="handleContactMouseLeave(europeRefsOffset + idx)"
        >
          <h3 class="text-2xl font-semibold text-[#848484] mb-4">{{ country.name }}</h3>
          <p class="text-[#848484] mb-6">{{ country.city }}</p>
          <div class="space-y-2 w-full flex flex-col items-center">
            <p class="text-sm text-[#848484]">Email</p>
            <div class="flex items-center justify-center gap-2">
              <a :href="`mailto:${country.email}`" class="text-[#848484] hover:underline">
                {{ country.email }}
              </a>
              <button
                type="button"
                class="ml-1 px-2 py-1 rounded bg-gray-100 hover:bg-gray-200 text-xs text-[#848484] border border-gray-300 transition"
                @click="copyEmail(country.email, europeRefsOffset + idx)"
              >
                <span v-if="copiedIdx === europeRefsOffset + idx && copied">Copied!</span>
                <span v-else>Copy</span>
              </button>
            </div>
          </div>
          <div
            class="contact-card-bg-characters pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[europeRefsOffset + idx] = el }"
          >
            {{ contactRandomTexts[europeRefsOffset + idx] }}
          </div>
        </div>
      </div>
    </div>

    <!-- Asia Section -->
    <div
      id="asia-section"
      ref="asiaSection"
      class="max-w-7xl mx-auto mb-12"
    >
      <h2 class="text-4xl font-bold text-[#848484] mb-8 ">Asia</h2>
      <div class="flex overflow-x-auto gap-0 no-scrollbar">
        <div
          v-for="(country, idx) in asiaCountries"
          :key="'asia-'+idx"
          class="contact-card flex flex-col items-center justify-center text-center"
          @mousemove="handleContactMouseMove($event, asiaRefsOffset + idx)"
          @mouseleave="handleContactMouseLeave(asiaRefsOffset + idx)"
          @touchmove="handleContactTouchMove($event, asiaRefsOffset + idx)"
          @touchend="handleContactMouseLeave(asiaRefsOffset + idx)"
        >
          <h3 class="text-2xl font-semibold text-[#848484] mb-4">{{ country.name }}</h3>
          <p class="text-[#848484] mb-6">{{ country.city }}</p>
          <div class="space-y-2 w-full flex flex-col items-center">
            <p class="text-sm text-[#848484]">Email</p>
            <div class="flex items-center justify-center gap-2">
              <a :href="`mailto:${country.email}`" class="text-[#848484] hover:underline">
                {{ country.email }}
              </a>
              <button
                type="button"
                class="ml-1 px-2 py-1 rounded bg-gray-100 hover:bg-gray-200 text-xs text-[#848484] border border-gray-300 transition"
                @click="copyEmail(country.email, asiaRefsOffset + idx)"
              >
                <span v-if="copiedIdx === asiaRefsOffset + idx && copied">Copied!</span>
                <span v-else>Copy</span>
              </button>
            </div>
          </div>
          <div
            class="contact-card-bg-characters pointer-events-none"
            :ref="el => { if (el) contactCharactersRefs[asiaRefsOffset + idx] = el }"
          >
            {{ contactRandomTexts[asiaRefsOffset + idx] }}
          </div>
        </div>
      </div>
    </div>

    <!-- Send us a message (Form) Section -->
    <div
      id="form-section"
      ref="formSection"
      class="max-w-7xl mx-auto mb-12"
    >
      <FormComponent />
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, onMounted, onUnmounted } from 'vue'
import FormComponent from '~/components/form.vue'
import worldc from '~/components/worldc.vue'

// Tabs
const tabs = [
  { key: 'middleEast', label: 'Middle East' },
  { key: 'cis', label: 'CIS & Caucasus' },
  { key: 'europe', label: 'Europe' },
  { key: 'asia', label: 'Asia' },
  { key: 'form', label: 'Send us a message' }
]

// Active tab
const activeTab = ref('middleEast')
const sectionRefs = {
  middleEast: ref(null),
  cis: ref(null),
  europe: ref(null),
  asia: ref(null),
  form: ref(null)
}

// Scroll to section
const scrollToSection = async (tabKey) => {
  activeTab.value = tabKey
  await nextTick()
  let sectionId = ''
  switch (tabKey) {
    case 'middleEast': sectionId = 'middleEast-section'; break
    case 'cis': sectionId = 'cis-section'; break
    case 'europe': sectionId = 'europe-section'; break
    case 'asia': sectionId = 'asia-section'; break
    case 'form': sectionId = 'form-section'; break
  }
  const el = document.getElementById(sectionId)
  if (el) {
    el.scrollIntoView({ behavior: 'smooth', block: 'start' })
  }
}

// Countries data
const middleEastCountries = [
  { name: 'Iraq', city: 'Baghdad', email: 'iraq@neverhack.com' },
  { name: 'Iran', city: 'Tehran', email: 'iran@neverhack.com' },
  { name: 'Kuwait', city: 'Kuwait City', email: 'kuwait@neverhack.com' }
]
const cisCountries = [
  { name: 'Russia', city: 'Moscow', email: 'russia@neverhack.com' },
  { name: 'Belarus', city: 'Minsk', email: 'belarus@neverhack.com' },
  { name: 'Kazakhstan', city: 'Astana', email: 'kazakhstan@neverhack.com' },
  { name: 'Kyrgyzstan', city: 'Bishkek', email: 'kyrgyzstan@neverhack.com' },
  { name: 'Tajikistan', city: 'Dushanbe', email: 'tajikistan@neverhack.com' },
  { name: 'Uzbekistan', city: 'Tashkent', email: 'uzbekistan@neverhack.com' },
  { name: 'Armenia', city: 'Yerevan', email: 'armenia@neverhack.com' },
  { name: 'Azerbaijan', city: 'Baku', email: 'azerbaijan@neverhack.com' }
]
const europeCountries = [
  { name: 'Turkey', city: 'Ankara', email: 'turkey@neverhack.com' },
  { name: 'Greece', city: 'Athens', email: 'greece@neverhack.com' }
]
const asiaCountries = [
  { name: 'China', city: 'Beijing', email: 'china@neverhack.com' },
  { name: 'India', city: 'New Delhi', email: 'india@neverhack.com' },
  { name: 'Pakistan', city: 'Islamabad', email: 'pakistan@neverhack.com' }
]

// Offsets for refs/texts
const countryRefsOffset = 0
const cisRefsOffset = countryRefsOffset + middleEastCountries.length
const europeRefsOffset = cisRefsOffset + cisCountries.length
const asiaRefsOffset = europeRefsOffset + europeCountries.length
const totalCards = middleEastCountries.length + cisCountries.length + europeCountries.length + asiaCountries.length

const contactCharactersRefs = ref([])
const CONTACT_PARTICLE_LENGTH_INITIAL = 95000
const CONTACT_PARTICLE_LENGTH_ON_MOVE = 95000
const contactRandomChemicalString = (length = CONTACT_PARTICLE_LENGTH_ON_MOVE) => {
  const pair = '· '
  const repeats = Math.ceil(length / pair.length)
  return pair.repeat(repeats).substring(0, length)
}
const contactRandomTexts = ref(
  Array(totalCards).fill().map(() => contactRandomChemicalString(CONTACT_PARTICLE_LENGTH_INITIAL))
)

const handleContactMouseMove = (e, index) => {
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top
  if (contactCharactersRefs.value[index]) {
    contactCharactersRefs.value[index].style.setProperty('--x', `${x}px`)
    contactCharactersRefs.value[index].style.setProperty('--y', `${y}px`)
    const cx = rect.width / 2
    const cy = rect.height / 2
    const maxOffset = 18
    const tx = ((x - cx) / cx) * maxOffset
    const ty = ((y - cy) / cy) * maxOffset
    contactCharactersRefs.value[index].style.setProperty('--tx', `${tx}px`)
    contactCharactersRefs.value[index].style.setProperty('--ty', `${ty}px`)
    contactCharactersRefs.value[index].style.setProperty('--scale', '1.06')
    contactRandomTexts.value[index] = contactRandomChemicalString(CONTACT_PARTICLE_LENGTH_ON_MOVE)
  }
}
const handleContactTouchMove = (e, index) => {
  const touch = e.touches[0]
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = touch.clientX - rect.left
  const y = touch.clientY - rect.top
  if (contactCharactersRefs.value[index]) {
    contactCharactersRefs.value[index].style.setProperty('--x', `${x}px`)
    contactCharactersRefs.value[index].style.setProperty('--y', `${y}px`)
    const cx = rect.width / 2
    const cy = rect.height / 2
    const maxOffset = 18
    const tx = ((x - cx) / cx) * maxOffset
    const ty = ((y - cy) / cy) * maxOffset
    contactCharactersRefs.value[index].style.setProperty('--tx', `${tx}px`)
    contactCharactersRefs.value[index].style.setProperty('--ty', `${ty}px`)
    contactCharactersRefs.value[index].style.setProperty('--scale', '1.06')
    contactRandomTexts.value[index] = contactRandomChemicalString(CONTACT_PARTICLE_LENGTH_ON_MOVE)
  }
}
const handleContactMouseLeave = (index) => {
  if (contactCharactersRefs.value[index]) {
    contactCharactersRefs.value[index].style.setProperty('--tx', `0px`)
    contactCharactersRefs.value[index].style.setProperty('--ty', `0px`)
    contactCharactersRefs.value[index].style.setProperty('--scale', '1')
  }
}

// Copy email logic
const copied = ref(false)
const copiedIdx = ref(null)
const copyEmail = async (email, idx) => {
  try {
    await navigator.clipboard.writeText(email)
    copied.value = true
    copiedIdx.value = idx
    setTimeout(() => {
      copied.value = false
      copiedIdx.value = null
    }, 1200)
  } catch (e) {
    // fallback: do nothing
  }
}

// Globe scroll-rotate state
const scrollRotate = ref(0)
const handleScroll = () => {
  // Use scrollTop to rotate the globe (adjust factor as needed)
  scrollRotate.value = window.scrollY * 0.003
}
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
/* Contact card hover effect styles (adapted from cards.vue) */
.contact-card-hover-container {
  position: relative;
  cursor: pointer;
  transition: transform 0.3s ease;
  border-radius: 0 !important;
}
.contact-card-hover-container:hover {
  /* Remove transform: translateY(-2px); */
}

/* Show background characters on hover, but don't scale card */
.contact-card-bg-characters {
  --x: 0px;
  --y: 0px;
  --tx: 0px;
  --ty: 0px;
  --scale: 1;
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  word-wrap: break-word;
  font-size: 20px;
  line-height: 1;
  overflow: hidden;
  font-family: monospace;
  color: #a8a8a8;
  opacity: 0;
  transition: opacity 0.45s cubic-bezier(.2,.9,.3,1);
  will-change: transform, opacity;
  -webkit-mask-image: radial-gradient(
    300px circle at var(--x) var(--y),
    #000 20%,
    rgba(0, 0, 0, 0.25),
    transparent
  );
  mask-image: radial-gradient(
    300px circle at var(--x) var(--y),
    #000 20%,
    rgba(0, 0, 0, 0.25),
    transparent
  );
  transform: translate(var(--tx, 0px), var(--ty, 0px)) scale(var(--scale, 1.025));
  padding: 20px;
  z-index: 2;
  pointer-events: none;
}
.contact-card:hover .contact-card-bg-characters,
.contact-card:active .contact-card-bg-characters {
  opacity: 1;
}

/* Remove transform/scale on card itself */
.contact-card {
  background: #fff;
  border-radius: 0;
  padding: 2rem;
  box-shadow: 0 1px 4px 0 #e5e7eb;
  position: relative;
  overflow: hidden;
  border-right: 1px solid #e5e7eb;
  min-width: 320px;
  min-height: 400px;
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  transition: box-shadow 0.3s;
}
.contact-card:hover {
  /* No transform or scale */
  box-shadow: 0 4px 16px 0 #e5e7eb;
}

/* Copy button hover style (like form) */
button[type="button"].ml-1 {
  position: relative;
  overflow: hidden;
  background: transparent;
  color: #848484;
  border: 1px solid #e5e7eb;
  transition: color 300ms, border-color 300ms;
  z-index: 1;
}
button[type="button"].ml-1::before {
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
button[type="button"].ml-1:hover::before,
button[type="button"].ml-1:focus-visible::before {
  transform: translateY(0);
}
button[type="button"].ml-1:hover,
button[type="button"].ml-1:focus-visible {
  color: #fff;
  border-color: #FFCD05;
  outline: none;
}
button[type="button"].ml-1 > span {
  position: relative;
  z-index: 1;
}

/* Ensure globe is behind header/tabs and only half visible below tabs */
@media (max-width: 700px) {
  .relative.flex.justify-center.items-center.w-full {
    height: 320px !important;
    min-height: 200px !important;
  }
  .absolute.left-1\/2.top-0.-translate-x-1\/2.z-0 {
    width: 100vw !important;
    height: 320px !important;
    min-width: 0 !important;
  }
  .absolute.left-1\/2.top-1\/2.z-10 h1 {
    font-size: 2.5rem !important;
  }
}
.no-scrollbar::-webkit-scrollbar {
  display: none;
}
.no-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* Iran flag animation styles */
.iran-flag-anim-container {
  width: 120px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 32px;
  margin-bottom: 8px;
  pointer-events: none;
}
.iran-flag-anim {
  width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 4px 16px 0 #e5e7eb;
  opacity: 0;
  transform: scale(0.92) rotate(-2deg);
  animation: iranFlagFadeIn 0.7s cubic-bezier(.2,.9,.3,1) forwards;
}
.contact-card:hover .iran-flag-anim,
.contact-card:active .iran-flag-anim {
  animation: iranFlagPop 0.5s cubic-bezier(.2,.9,.3,1) forwards;
}
@keyframes iranFlagFadeIn {
  to {
    opacity: 1;
    transform: scale(1) rotate(0deg);
  }
}
@keyframes iranFlagPop {
  0% {
    opacity: 0.8;
    transform: scale(0.92) rotate(-2deg);
  }
  60% {
    opacity: 1;
    transform: scale(1.08) rotate(2deg);
  }
  100% {
    opacity: 1;
    transform: scale(1) rotate(0deg);
  }
}

/* پس‌زمینه پرچم ایران با افکت موجی */
.iran-flag-bg {
  --x: 0px;
  --y: 0px;
  --tx: 0px;
  --ty: 0px;
  --scale: 1;
  position: absolute;
  inset: 0;
  z-index: 0;
  opacity: 0;
  pointer-events: none;
  background: url('https://upload.wikimedia.org/wikipedia/commons/c/ca/Flag_of_Iran.svg') center center/cover no-repeat;
  transition: opacity 0.5s cubic-bezier(.2,.9,.3,1);
  border-radius: 0;
  /* افکت موجی */
  -webkit-mask-image: radial-gradient(
    300px circle at var(--x) var(--y),
    #000 20%,
    rgba(0, 0, 0, 0.25),
    transparent
  );
  mask-image: radial-gradient(
    300px circle at var(--x) var(--y),
    #000 20%,
    rgba(0, 0, 0, 0.25),
    transparent
  );
  transform: translate(var(--tx, 0px), var(--ty, 0px)) scale(var(--scale, 1.025));
  will-change: transform, opacity;
}
.contact-card:hover .iran-flag-bg,
.contact-card:active .iran-flag-bg {
  opacity: 0.18;
}

/* Iraq flag background with diagonal wipe effect (shows on hover) */
.iraq-flag-bg {
  --x: 0px;
  --y: 0px;
  --tx: 0px;
  --ty: 0px;
  --scale: 1;
  position: absolute;
  inset: 0;
  z-index: 0;
  opacity: 0;
  pointer-events: none;
  background: url('https://upload.wikimedia.org/wikipedia/commons/f/f6/Flag_of_Iraq.svg') center center/cover no-repeat;
  border-radius: 0;
  transition: opacity 0.5s cubic-bezier(.2,.9,.3,1), clip-path 0.6s cubic-bezier(.2,.9,.3,1);
  /* افکت مورب */
  clip-path: polygon(100% 0, 100% 0, 100% 100%, 100% 100%);
  will-change: transform, opacity, clip-path;
}
.contact-card:hover .iraq-flag-bg,
.contact-card:active .iraq-flag-bg {
  opacity: 0.18;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
}
</style>