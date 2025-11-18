<template>
  <section class="chemistry-cards-section">
    <div class="container">
      <div class="cards-container">
        <div class="row">
          <div class="col-lg-4" v-for="(card, index) in cards" :key="index">
            <div class="single-card-wrapper">
              <a :href="card.link">
                <div 
                  class="card-hover-container"
                  @mousemove="handleMouseMove($event, index)"
                  @touchmove="handleTouchMove($event, index)"
                >
                  <img :src="card.icon" :alt="card.alt">
                  <div class="card-gradient"></div>
                  <div 
                    class="card-bg-characters"
                    :ref="el => { if (el) charactersRefs[index] = el }"
                  >
                    {{ randomTexts[index] }}
                  </div>
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
import { ref, onMounted } from 'vue'

// تعریف کارت‌ها
const cards = ref([
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//github-mark-white.svg',
    alt: 'Chemistry 1',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//code.svg',
    alt: 'Chemistry 2',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//die.svg',
    alt: 'Chemistry 3',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//github-mark-white.svg',
    alt: 'Chemistry 4',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//code.svg',
    alt: 'Chemistry 5',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//die.svg',
    alt: 'Chemistry 6',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//github-mark-white.svg',
    alt: 'Chemistry 7',
    link: '#'
  },
  {
    icon: 'https://www.yudiz.com/codepen/code-hover//code.svg',
    alt: 'Chemistry 8',
    link: '#'
  }
])

// معادلات شیمیایی
const chemicalEquations = [
  '2H₂ + O₂ → 2H₂O',
  'CH₄ + 2O₂ → CO₂ + 2H₂O',
  'N₂ + 3H₂ → 2NH₃',
  'CaCO₃ → CaO + CO₂',
  '2Na + Cl₂ → 2NaCl',
  'H₂SO₄ + 2NaOH → Na₂SO₄ + 2H₂O',
  'Fe + S → FeS',
  'C₆H₁₂O₆ + 6O₂ → 6CO₂ + 6H₂O',
  '2KClO₃ → 2KCl + 3O₂',
  'Zn + 2HCl → ZnCl₂ + H₂',
  'Mg + O₂ → MgO',
  'AgNO₃ + NaCl → AgCl + NaNO₃',
  'C₃H₈ + 5O₂ → 3CO₂ + 4H₂O',
  '2Al + 3Cl₂ → 2AlCl₃',
  'Cu + 2AgNO₃ → Cu(NO₃)₂ + 2Ag',
  'NH₄NO₃ → N₂O + 2H₂O',
  'C₂H₅OH + 3O₂ → 2CO₂ + 3H₂O',
  'Ca(OH)₂ + CO₂ → CaCO₃ + H₂O',
  '2Mg + O₂ → 2MgO',
  'PCl₅ → PCl₃ + Cl₂'
]

const charactersRefs = ref([])
const randomTexts = ref(['', '', ''])

// تولید رشته تصادفی از معادلات شیمیایی
const randomChemicalString = (length = 2000) => {
  let result = ''
  while (result.length < length) {
    const randomEquation = chemicalEquations[Math.floor(Math.random() * chemicalEquations.length)]
    result += randomEquation + '  '
  }
  return result.substring(0, length)
}

// مدیریت حرکت موس
const handleMouseMove = (e, index) => {
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top
  
  if (charactersRefs.value[index]) {
    charactersRefs.value[index].style.setProperty('--x', `${x}px`)
    charactersRefs.value[index].style.setProperty('--y', `${y}px`)
    randomTexts.value[index] = randomChemicalString(2000)
  }
}

// مدیریت لمس
const handleTouchMove = (e, index) => {
  const touch = e.touches[0]
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = touch.clientX - rect.left
  const y = touch.clientY - rect.top
  
  if (charactersRefs.value[index]) {
    charactersRefs.value[index].style.setProperty('--x', `${x}px`)
    charactersRefs.value[index].style.setProperty('--y', `${y}px`)
    randomTexts.value[index] = randomChemicalString(2000)
  }
}
</script>

<style scoped>
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
  background: #000;
  font-family: Arial, Helvetica, sans-serif;
  padding: 20px;
}

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}

.cards-container {
  position: relative;
  width: 100%;
}

.row {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
}

.col-lg-4 {
  flex: 1;
  min-width: 280px;
  max-width: 400px;
}

.single-card-wrapper > a {
  display: block;
  height: 100%;
  width: 100%;
  color: #fff;
  font-weight: 500;
  text-decoration: underline solid #5e6077 1px;
  transition: color 0.2s cubic-bezier(0.45, 0, 0.55, 1);
  text-underline-offset: 3px;
}

.single-card-wrapper > a:hover {
  color: #ececfb;
}

.card-hover-container {
  border: 0.5px solid #ffffff1e;
  width: 100%;
  border-radius: 26px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
  cursor: pointer;
  min-height: 399px;
  aspect-ratio: 1;
}

.card-gradient {
  height: 100%;
  width: 100%;
  position: absolute;
  background: radial-gradient(#171825 40%, #63e 50%, #8e64ff, #f92672);
  mix-blend-mode: darken;
  pointer-events: none;
  z-index: 3;
}

.card-hover-container > img {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  z-index: 4;
  max-width: 80px;
}

.card-bg-characters {
  --x: 0px;
  --y: 0px;
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  word-wrap: break-word;
  font-size: 14px;
  overflow: hidden;
  font-family: monospace;
  line-height: 1.15;
  color: #fff;
  opacity: 0;
  transition: 0.5s;
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
  transform: scale(1.025);
  padding: 20px;
}

.card-hover-container:hover .card-bg-characters {
  opacity: 1;
}

@media (max-width: 991px) {
  .col-lg-4 {
    min-width: 250px;
  }
  
  .card-hover-container {
    min-height: 300px;
  }
}
</style>