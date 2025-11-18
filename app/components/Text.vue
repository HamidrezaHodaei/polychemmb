<template>
  <div class="min-h-screen w-full flex justify-center items-center" style="background-color: #f1f2f2;">
    <div ref="textEl" class="font-mono font-thin text-5xl text-[#848484]"></div>
  </div>
  <div><p>zz</p></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const textEl = ref(null)

class TextScramble {
  constructor(el) {
    this.el = el
    this.chars = '!<>-_\\/[]{}—=+*^?#________'
    this.update = this.update.bind(this)
  }
  
  setText(newText) {
    const oldText = this.el.innerText
    const length = Math.max(oldText.length, newText.length)
    const promise = new Promise((resolve) => this.resolve = resolve)
    this.queue = []
    for (let i = 0; i < length; i++) {
      const from = oldText[i] || ''
      const to = newText[i] || ''
      const start = Math.floor(Math.random() * 40)
      const end = start + Math.floor(Math.random() * 40)
      this.queue.push({ from, to, start, end })
    }
    cancelAnimationFrame(this.frameRequest)
    this.frame = 0
    this.update()
    return promise
  }
  
  update() {
    let output = ''
    let complete = 0
    for (let i = 0, n = this.queue.length; i < n; i++) {
      let { from, to, start, end, char } = this.queue[i]
      if (this.frame >= end) {
        complete++
        output += to
      } else if (this.frame >= start) {
        if (!char || Math.random() < 0.28) {
          char = this.randomChar()
          this.queue[i].char = char
        }
        output += `<span class="text-yellow-400">${char}</span>`
      } else {
        output += from
      }
    }
    this.el.innerHTML = output
    if (complete === this.queue.length) {
      this.resolve()
    } else {
      this.frameRequest = requestAnimationFrame(this.update)
      this.frame++
    }
  }
  
  randomChar() {
    return this.chars[Math.floor(Math.random() * this.chars.length)]
  }
}

const phrases = [
  'Polymer Art',
  'Originality',
  'Loyalty',
  'Yearn',
  'Creativity',
  'Honesty',
  'Elegance',
  'Meaningful'
]

let fx = null
let counter = 0
let timeoutId = null

const next = () => {
  fx.setText(phrases[counter]).then(() => {
    timeoutId = setTimeout(next, 800)
  })
  counter = (counter + 1) % phrases.length
}

onMounted(() => {
  if (textEl.value) {
    fx = new TextScramble(textEl.value)
    next()
  }
})

onUnmounted(() => {
  if (timeoutId) {
    clearTimeout(timeoutId)
  }
  if (fx && fx.frameRequest) {
    cancelAnimationFrame(fx.frameRequest)
  }
})
</script>

<style scoped>

</style>