<template>
  <div class="my-6">
    <section
  ref="whyUsSection"
  id="whyUsSection"
  class="my-40 my-md-0 px-4 py-16 bg-gradient-to-br from-gray-50 to-blue-50 flex flex-col items-center justify-center"
>
      <!-- Section Title -->
      <h3 ref="sectionTitle" class="text-center text-3xl sm:text-4xl text-blue-500 font-bold mb-10">Why Us</h3>

      <!-- Reason Cards -->
      <ul ref="reasonsList" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl">
        <li
          v-for="(reason, index) in reasons"
          :key="index"
          ref="reasonCards"
          class="bg-white shadow-lg p-6 rounded-xl hover:shadow-2xl transition-all duration-300 cursor-pointer border border-gray-100"
        >
          <div ref="iconContainers" class="w-12 h-12 bg-gradient-to-r from-blue-500 to-blue-600 rounded-full flex items-center justify-center mb-4 mx-auto">
            <div class="w-6 h-6 bg-white rounded-full"></div>
          </div>
          <h4 ref="reasonTitles" class="text-blue-600 font-bold text-lg mb-2 text-center">{{ reason.title }}</h4>
          <p ref="reasonDescriptions" class="text-gray-700 text-sm text-center">{{ reason.description }}</p>
        </li>
      </ul>

      <!-- Testimoni -->
      <div class="mt-20 max-w-3xl text-center">
        <p class="text-xl sm:text-2xl text-gray-600 italic">"They delivered everything on time with excellent support. Couldn't be happier!"</p>
        <p class="mt-4 text-blue-600 font-bold">— Happy Client</p>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, nextTick } from 'vue'
import { useTemplateRefsList } from '@vueuse/core'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

// Data
const reasons = [
  { title: 'Experienced Team', description: 'We bring years of experience in web and mobile development.' },
  { title: 'Client-Centered Approach', description: 'We prioritize your vision and goals in every project.' },
  { title: 'Full-Cycle Services', description: 'From design to deployment, we handle everything.' },
  { title: 'Latest Technology', description: 'We use modern frameworks and tools.' },
  { title: 'Quality Assurance', description: 'Every product is tested for perfection.' },
  { title: 'Reliable Support', description: 'We provide ongoing support and maintenance.' },
]

// Refs
const whyUsSection = ref<HTMLElement | null>(null)
const sectionTitle = ref<HTMLElement | null>(null)
const reasonsList = ref<HTMLElement | null>(null)

const reasonCards = useTemplateRefsList<HTMLElement>()
const iconContainers = useTemplateRefsList<HTMLElement>()
const reasonTitles = useTemplateRefsList<HTMLElement>()
const reasonDescriptions = useTemplateRefsList<HTMLElement>()

// Mounted
onMounted(() => {
  nextTick(() => {
    const screenIsSmall = window.innerWidth < 640

    // Section title animation
    gsap.from(sectionTitle.value, {
      opacity: 0,
      y: -40,
      duration: 1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionTitle.value,
        start: 'top 85%',
      }
    })

    // Cards entrance animation (staggered)
    reasonCards.value.forEach((card, index) => {
      gsap.from(card, {
        opacity: 0,
        y: 80,
        scale: 0.9,
        duration: 0.6,
        delay: screenIsSmall ? index * 0.15 : index * 0.1,
        ease: 'power2.out',
        scrollTrigger: {
          trigger: card,
          start: 'top 85%',
          toggleActions: 'play none none reverse'
        }
      })
    })

    // Floating & Pulse & Hover
    reasonCards.value.forEach((card, index) => {
      gsap.to(card, {
        y: -6,
        duration: 2 + index * 0.1,
        repeat: -1,
        yoyo: true,
        ease: 'sine.inOut',
        delay: index * 0.1
      })

      iconContainers.value[index] && gsap.to(iconContainers.value[index], {
        scale: 1.1,
        duration: 1.5,
        ease: 'power2.inOut',
        yoyo: true,
        repeat: -1,
        delay: index * 0.1
      })

      // Hover effects
      card.addEventListener('mouseenter', () => {
        gsap.to(card, {
          scale: 1.05,
          rotationY: 5,
          duration: 0.4,
          ease: 'power2.out'
        })
        reasonTitles.value[index] && gsap.to(reasonTitles.value[index], {
          color: '#1d4ed8',
          scale: 1.05,
          duration: 0.3,
        })
        reasonDescriptions.value[index] && gsap.to(reasonDescriptions.value[index], {
          y: -2,
          duration: 0.3,
        })
        iconContainers.value[index] && gsap.to(iconContainers.value[index], {
          rotation: 180,
          scale: 1.2,
          duration: 0.4,
        })
      })

      card.addEventListener('mouseleave', () => {
        gsap.to(card, {
          scale: 1,
          rotationY: 0,
          duration: 0.4,
          ease: 'power2.out'
        })
        reasonTitles.value[index] && gsap.to(reasonTitles.value[index], {
          color: '#2563eb',
          scale: 1,
          duration: 0.3,
        })
        reasonDescriptions.value[index] && gsap.to(reasonDescriptions.value[index], {
          y: 0,
          duration: 0.3,
        })
        iconContainers.value[index] && gsap.to(iconContainers.value[index], {
          rotation: 0,
          scale: 1.1,
          duration: 0.4,
        })
      })
    })

    // Parallax section effect
    gsap.to(whyUsSection.value, {
      yPercent: -10,
      ease: 'none',
      scrollTrigger: {
        trigger: whyUsSection.value,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 1,
      }
    })
  })
})
</script>

<style scoped>
/* Optional: smooth out transitions */
</style>
