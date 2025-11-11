<template>
  
  <div class="min-h-screen flex items-center justify-center bg-[#f1f2f2] p-6 font-sans relative text-[#808285]">
    <!-- Top-left Logo -->
    <div class="absolute top-4 left-4 z-20">
      <img src="/polychem wall B-0۱.png" alt="Logo" class="w-40 h-auto" />
    </div>
    <div class="bg-white rounded-xl shadow-lg h-[640px] max-w-5xl w-full flex flex-col md:flex-row overflow-hidden md:h-[580px]">
      
      <!-- Left Image (Desktop only) -->
    <div class="hidden md:block md:flex-1 h-full w-[600px]">
        <img src="/white-plastic-polymer.jpg" alt="Form Image" class="w-full h-full object-cover" />
      </div>

      <!-- Form -->
      <form class="w-full md:w-[420px] p-8 flex flex-col items-center justify-center h-full pt-0 relative">
        <!-- Back to Home -->
        <a href="/" aria-label="Back to home" class="absolute top-4 right-4 text-gray-500 hover:text-gray-700 text-2xl leading-none">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7"/>
          </svg>
        </a>
        <!-- Logo & Heading -->
        <div class="text-center mb-8">
          <h1 class="text-2xl font-semibold mb-2">Welcome Back!</h1>
          <p>Sign in to access your dasboard.</p>
        </div>

        <!-- Input Fields -->
        <div class="w-full max-w-[420px] flex flex-col gap-6">
          <div class="relative">
            <input required type="text" id="username" placeholder=" " class="peer w-full border-b-2 border-gray-300 focus:border-[#FFCD05] outline-none py-2 bg-white" />
            <label for="username" class="absolute left-0 top-2 text-gray-400 text-sm transition-all
              peer-placeholder-shown:top-2 peer-placeholder-shown:text-gray-400 peer-placeholder-shown:text-sm
              peer-focus:-top-4 peer-focus:text-[#FFCD05] peer-focus:text-sm
              peer-[:not(:placeholder-shown)]:-top-4 peer-[:not(:placeholder-shown)]:text-gray-700 peer-[:not(:placeholder-shown)]:text-sm">Username</label>
          </div>

          <div class="relative">
            <input required :type="showPassword ? 'text' : 'password'" id="password" placeholder=" " class="peer w-full border-b-2 border-gray-300 focus:border-[#FFCD05] outline-none py-2 bg-white pr-10" />
            <label for="password" class="absolute left-0 top-2 text-gray-400 text-sm transition-all
              peer-placeholder-shown:top-2 peer-placeholder-shown:text-gray-400 peer-placeholder-shown:text-sm
              peer-focus:-top-4 peer-focus:text-[#FFCD05] peer-focus:text-sm
              peer-[:not(:placeholder-shown)]:-top-4 peer-[:not(:placeholder-shown)]:text-gray-700 peer-[:not(:placeholder-shown)]:text-sm">Password</label>
            <!-- Password visibility toggle -->
            <button
              type="button"
              @click="showPassword = !showPassword"
              class="absolute right-0 top-1/2 -translate-y-1/2 p-2 text-gray-500 hover:text-gray-700 focus-visible:outline-none"
              aria-label="Toggle password visibility"
            >
              <!-- Eye / Eye-off icon -->
              <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7-10-7-10-7z"/>
                <circle cx="12" cy="12" r="3"/>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M3 3l18 18"/>
                <path d="M10.6 10.6a3 3 0 004.2 4.2"/>
                <path d="M9.88 4.26A10.94 10.94 0 0112 4c6.5 0 10 8 10 8a17.44 17.44 0 01-3.36 4.58M6.61 6.61A17.53 17.53 0 002 12s3.5 7 10 7a10.82 10.82 0 004.39-.93"/>
              </svg>
            </button>
          </div>
          <a href="#" class="hover:text-[#FFCD05] text-sm">Forgot your password?</a>
          <button type="submit" class="btn-slide-down w-full h-12 rounded-lg relative overflow-hidden border-2 border-[#FFCD05] text-[#FFCD05] transition-colors">
            <span class="relative z-10">Sign in</span>
          </button>
        </div>

        <!-- Footer links -->
        <div class="w-full max-w-[480px] mt-6 text-center text-sm text-gray-600 relative pb-10">
          <p>Don't have an account? <a href="/register" class="text-[#FFCD05] hover:text-[#808285]">Sign up</a></p>
          <div class="absolute right-0 -bottom-12 text-right">
            <div class="inline-flex items-center gap-2 cursor-pointer select-none">
              <span class="">English</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M6 9l6 6 6-6"/>
              </svg>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
 import Navbar from "~/components/navbar.vue"
import { ref } from 'vue'
const showPassword = ref(false)
</script>

<style scoped>
/* اطمینان از سفید بودن پس‌زمینه‌ی ورودی‌ها (حتی در حالت autofill مرورگر) */
input {
  background-color: #ffffff !important;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus {
  -webkit-box-shadow: 0 0 0 1000px #ffffff inset !important;
  box-shadow: 0 0 0 1000px #ffffff inset !important;
  -webkit-text-fill-color: #111111 !important;
  caret-color: #111111;
  transition: background-color 5000s ease-in-out 0s;
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
</style>
