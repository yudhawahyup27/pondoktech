<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

// State toggle mobile menu
const isOpen = ref(false)

// Scroll effect: navbar shadow + background change
const scrolled = ref(false)

const handleScroll = () => {
  scrolled.value = window.scrollY > 20
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})
onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll)
})

function toggleMenu() {
  isOpen.value = !isOpen.value
}
</script>

<template>
  <nav
    :class="[
      'fixed top-0 left-0 w-full z-50 transition-all duration-300',
      scrolled ? 'bg-white shadow-md' : 'bg-transparent',
    ]"
  >
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <!-- Logo -->
        <div class="flex-shrink-0 flex items-center">

          <img src="/logo.png" class="w-10" alt="">
          <a href="#" class="text-2xl font-bold text-blue-600 select-none">
           Pondok Tech
          </a>
        </div>

        <!-- Desktop Menu -->
        <div class="hidden md:flex space-x-8">
          <a
            href="#"
            class="text-gray-700 hover:text-blue-600 font-medium transition"
            >Home</a
          >
          <a
            href="#serviceSection"
            class="text-gray-700 hover:text-blue-600 font-medium transition"
            >Services</a
          >
          <a
            href="#whyUsSection"
            class="text-gray-700 hover:text-blue-600 font-medium transition"
            >Why Us</a
          >
          <a
            href="#testimoniSection"
            class="text-gray-700 hover:text-blue-600 font-medium transition"
            >Testimonials</a
          >
          <a
            href="#portofolioProject"
            class="text-gray-700 hover:text-blue-600 font-medium transition"
            >Portfolio</a
          >
        </div>

        <!-- Mobile Hamburger -->
        <div class="md:hidden">
          <button
            @click="toggleMenu"
            type="button"
            aria-controls="mobile-menu"
            :aria-expanded="isOpen"
            class="inline-flex items-center justify-center p-2 rounded-md text-gray-700 hover:text-blue-600 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-blue-500"
          >
            <span class="sr-only">Open main menu</span>
            <svg
              v-if="!isOpen"
              class="block h-6 w-6"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              aria-hidden="true"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 6h16M4 12h16M4 18h16"
              />
            </svg>
            <svg
              v-else
              class="block h-6 w-6"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              aria-hidden="true"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Mobile Menu -->
    <div
      v-show="isOpen"
      id="mobile-menu"
      class="md:hidden bg-white shadow-md"
      @click.outside="isOpen = false"
    >
      <div class="px-2 pt-2 pb-3 space-y-1">
        <a
          href="#"
          @click="isOpen = false"
          class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:text-blue-600 hover:bg-gray-100 transition"
          >Home</a
        >
        <a
          href="#serviceSection"
          @click="isOpen = false"
          class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:text-blue-600 hover:bg-gray-100 transition"
          >Services</a
        >
        <a
          href="#whyUsSection"
          @click="isOpen = false"
          class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:text-blue-600 hover:bg-gray-100 transition"
          >Why Us</a
        >
        <a
          href="#testimoniSection"
          @click="isOpen = false"
          class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:text-blue-600 hover:bg-gray-100 transition"
          >Testimonials</a
        >
        <a
          href="#portofolioProject"
          @click="isOpen = false"
          class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 hover:text-blue-600 hover:bg-gray-100 transition"
          >Portfolio</a
        >
      </div>
    </div>
  </nav>
</template>

<style scoped>
/* Optional: smooth transition for background & shadow */
nav {
  transition-property: background-color, box-shadow;
  transition-duration: 0.3s;
  transition-timing-function: ease-in-out;
}
</style>
