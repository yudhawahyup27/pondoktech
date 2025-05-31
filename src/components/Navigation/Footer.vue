<template>
  <footer class=" text-blue-500 py-8 mt-12">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex flex-col md:flex-row justify-between items-center space-y-6 md:space-y-0">

        <!-- Logo dan Nama -->
        <div class="flex items-center space-x-3">
          <img src="/logo.png" alt="Logo" class="h-40 w-auto" />

        </div>

        <!-- Navigasi Footer -->
        <nav class="flex space-x-2 text-blue-500 text-sm">
          <a href="#serviceSection" class="hover:text-white transition">Services</a>
          <a href="#whyUsSection" class="hover:text-white transition">Why Us</a>
          <a href="#testimoniSection" class="hover:text-white transition">Testimonials</a>
          <a href="#portofolioProject" class="hover:text-white transition">Portfolio</a>
        </nav>

        <!-- Sosial Media -->
        <div class="flex space-x-4">
          <a href="https://www.instagram.com/pondoktech" target="_blank" rel="noopener" aria-label="Facebook" class="hover:text-white transition">
            <img class="w-20" src="https://img.icons8.com/?size=100&id=TEYr8ETaIfBJ&format=png&color=000000" alt="">
          </a>
          <a href="https://www.linkedin.com/company/pondok-tech" target="_blank" rel="noopener" aria-label="LinkedIn" class="hover:text-white transition">
            <img class="w-20" src="https://img.icons8.com/?size=100&id=108812&format=png&color=000000" alt="">
          </a>
        </div>
      </div>

      <div class="mt-8 text-center text-xs text-gray-500">
        &copy; {{ currentYear }} Pondok Tech. All rights reserved.
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import {ref} from"vue"

const currentYear = new Date().getFullYear()
</script>
