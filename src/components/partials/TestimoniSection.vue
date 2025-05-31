<script setup lang="ts">
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'

const testimonials = [
  {
    name: 'John Doe',
    role: 'CEO of ABC Corp',
    quote: 'They delivered a high-quality solution on time. Highly recommended!',
    image: 'https://i.pravatar.cc/100?img=1',
  },
  {
    name: 'Jane Smith',
    role: 'Product Manager',
    quote: 'Working with them was seamless and efficient.',
    image: 'https://i.pravatar.cc/100?img=2',
  },
  {
    name: 'Michael Johnson',
    role: 'CTO of XYZ Inc',
    quote: 'Very professional team. Great communication and results.',
    image: 'https://i.pravatar.cc/100?img=3',
  },
  {
    name: 'Emily Davis',
    role: 'Designer',
    quote: 'Creative and fast turnaround. Loved the design quality!',
    image: 'https://i.pravatar.cc/100?img=4',
  },
  {
    name: 'Robert Wilson',
    role: 'Developer',
    quote: 'Clean code and great architecture.',
    image: 'https://i.pravatar.cc/100?img=5',
  },
  {
    name: 'Lisa Brown',
    role: 'Marketing Head',
    quote: 'Helped us boost our online presence significantly.',
    image: 'https://i.pravatar.cc/100?img=6',
  },
]
</script>

<template>
  <div class="mx-auto px-4" id="testimoniSection">
    <h2 class="text-center font-bold text-xl text-blue-500 mb-6">Testimoni</h2>
    <Swiper

      :space-between="24"
      :slides-per-view="1"
      :autoplay="{
        delay: 3000,
      }"
      :breakpoints="{
        640: { slidesPerView: 1 },
        768: { slidesPerView: 2 },
        1024: { slidesPerView: 3 }
      }"
      class="rounded-lg"
    >
      <SwiperSlide
        v-for="(testimonial, i) in testimonials"
        :key="i"
        class="bg-white shadow-lg rounded-xl p-6 flex flex-col items-center text-center"
        style="width: 300px; height: 200px;"
      >
        <img :src="testimonial.image" alt="Photo" class="w-16 h-16 rounded-full mb-4" />
        <p class="italic text-gray-700 mb-2">"{{ testimonial.quote }}"</p>
        <h4 class="font-bold text-blue-500">{{ testimonial.name }}</h4>
        <span class="text-sm text-gray-500">{{ testimonial.role }}</span>
      </SwiperSlide>
    </Swiper>
  </div>
</template>
