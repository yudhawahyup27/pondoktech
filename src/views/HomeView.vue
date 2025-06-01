<script setup lang="ts">
import { onMounted, ref, nextTick } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import BannerHero from '../components/partials/BannerHero.vue'
import WhyUs from '../components/partials/WhyUs.vue'
import TestimoniSection from '../components/partials/TestimoniSection.vue'
import PortofolioProject from '../components/partials/PortofolioProject.vue'
// register ScrollTrigger plugin
gsap.registerPlugin(ScrollTrigger)

// Data services dengan tambahan deskripsi dan icon yang lebih konsisten
const services = [
  {
    title: 'Web & Mobile Development',
    icon: 'https://img.icons8.com/color/48/000000/source-code.png',
    desc: 'Custom aplikasi mobile & web dengan teknologi terbaru.'
  },
  {
    title: 'ERP, CMS & E-Commerce',
    icon: 'https://img.icons8.com/color/48/000000/shop.png',
    desc: 'Solusi manajemen bisnis & toko online.'
  },
  {
    title: 'UI/UX Design',
    icon: 'https://img.icons8.com/color/48/000000/design.png',
    desc: 'Desain antarmuka menarik & user friendly.'
  },
  {
    title: 'Testing & QA',
    icon: 'https://img.icons8.com/color/48/000000/test-passed.png',
    desc: 'Jaminan kualitas software bebas bug.'
  },
  {
    title: 'IoT & Networking',
    icon: 'https://img.icons8.com/?size=100&id=QZeIpGjrMtTZ&format=png&color=000000',
    desc: 'Integrasi perangkat pintar dan jaringan efisien.'
  },
  {
    title: 'IT Training',
    icon: 'https://img.icons8.com/color/48/000000/training.png',
    desc: 'Pelatihan profesional bidang teknologi.'
  }
]

// refs
const serviceSection = ref<HTMLElement | null>(null)
const whyUsSection = ref<HTMLElement | null>(null)
const testimoniSection = ref<HTMLElement | null>(null)
const maskotImg = ref<HTMLElement | null>(null)

// Untuk menyimpan elemen kartu layanan dengan push agar reaktif & aman
const serviceCards = ref<HTMLElement[]>([])
const addServiceCard = (el: HTMLElement | null) => {
  if (el && !serviceCards.value.includes(el)) {
    serviceCards.value.push(el)
  }
}

const waitForServiceCards = async () => {
  await nextTick()
  return new Promise<void>((resolve) => {
    requestAnimationFrame(() => resolve())
  })
}

onMounted(async () => {
  await waitForServiceCards()

  // Timeline animasi service section
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: serviceSection.value,
      start: 'top 85%',
      end: 'bottom 20%',
      toggleActions: 'play none none reverse',
    }
  })

  tl.from(maskotImg.value, {
    opacity: 0,
    scale: 0.5,
    rotation: -10,
    duration: 0.8,
    ease: 'back.out(1.7)'
  })

  tl.from(serviceCards.value, {
    opacity: 0,
    y: 80,
    scale: 0.8,
    rotation: 5,
    duration: 0.6,
    ease: 'power2.out',
    stagger: {
      amount: 0.8,
      from: 'start'
    }
  }, '-=0.4')

  // Animasi hover dan floating pada kartu service
  serviceCards.value.forEach((card, index) => {
    gsap.to(card, {
      y: -5,
      duration: 2,
      ease: 'power2.inOut',
      yoyo: true,
      repeat: -1,
      delay: index * 0.2
    })

    card.addEventListener('mouseenter', () => {
      gsap.to(card, {
        scale: 1.05,
        rotation: 2,
        duration: 0.3,
        ease: 'power2.out'
      })
    })

    card.addEventListener('mouseleave', () => {
      gsap.to(card, {
        scale: 1,
        rotation: 0,
        duration: 0.3,
        ease: 'power2.out'
      })
    })
  })

  // Animasi why us section dengan parallax scroll
  gsap.from(whyUsSection.value, {
    opacity: 0,
    y: 100,
    rotationX: 15,
    duration: 1.2,
    ease: 'power3.out',
    scrollTrigger: {
      trigger: whyUsSection.value,
      start: 'top 80%',
      end: 'bottom 20%',
      toggleActions: 'play none none reverse',
      onEnter: () => {
        gsap.to(whyUsSection.value, {
          yPercent: -20,
          ease: 'none',
          scrollTrigger: {
            trigger: whyUsSection.value,
            start: 'top bottom',
            end: 'bottom top',
            scrub: 1
          }
        })
      }
    }
  })

  // Animasi testimoni
  const testimoniTl = gsap.timeline({
    scrollTrigger: {
      trigger: testimoniSection.value,
      start: 'top 75%',
      end: 'bottom 25%',
      toggleActions: 'play none none reverse',
    }
  })

  testimoniTl
    .from(testimoniSection.value, {
      opacity: 0,
      scale: 0.7,
      rotationY: 15,
      duration: 1,
      ease: 'power2.out'
    })
    .to(testimoniSection.value, {
      scale: 1.02,
      duration: 0.3,
      ease: 'power2.out'
    })
    .to(testimoniSection.value, {
      scale: 1,
      duration: 0.3,
      ease: 'power2.out'
    })

  gsap.to(testimoniSection.value, {
    rotation: 2,
    ease: 'none',
    scrollTrigger: {
      trigger: testimoniSection.value,
      start: 'top bottom',
      end: 'bottom top',
      scrub: 2
    }
  })
})
</script>

<template>
  <div >
    <BannerHero  />

    <!-- SERVICE SECTION -->
     <div class="text-center  text-blue-500 font-bold mb-10 "  id="serviceSection">
      <h3 class="text-3xl">Our Services</h3>
      <p>We are not just a vendor. We are your strategic partner — not just building software, but building solutions together to create real value and enable impactful and sustainable digital transformation.</p>
     </div>
    <section
      ref="serviceSection"

      class="flex flex-col justify-center items-center mx-auto text-center  mb-24 "
    >
      <div class="md:flex justify-center mt-4 gap-8 items-center">
        <div class="hidden md:flex justify-center flex-shrink-0">
          <img ref="maskotImg" src="/maskot.png" alt="Maskot" class=" h-auto select-none" />
        </div>

        <div
          class="grid grid-cols-2 lg:grid-cols-3 gap-8 my-4 items-stretch max-w-4xl"
        >
          <div
            v-for="(service, index) in services"
            :key="index"
            ref="addServiceCard"
            class="p-6 rounded-xl shadow-md bg-white  flex flex-col justify-center items-center h-full cursor-pointer transition-all duration-300 hover:shadow-xl hover:scale-105"
          >
            <div
              class="bg-blue-100 rounded-full w-16 h-16 p-2 flex items-center justify-center mx-auto mb-4"
            >
              <img
                :src="service.icon"
                class="w-10 h-10"
                alt="Icon"
                loading="lazy"
                draggable="false"
              />
            </div>
            <h3 class="text-lg font-semibold text-blue-600 text-center">
              {{ service.title }}
            </h3>
            <p class="text-sm text-gray-600 dark:text-gray-300 text-center mt-2 leading-snug">
              {{ service.desc }}
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- WHY US SECTION -->
  <section ref="whyUsSection" class="my-24 px-4 lg:px-8 max-w-6xl mx-auto">
  <WhyUs />
</section>

    <section class="mx-auto px-4">
  <PortofolioProject />
</section>

    <!-- TESTIMONI SECTION -->
    <section ref="testimoniSection" class=" mx-auto px-4">
      <TestimoniSection />
    </section>
  </div>
</template>

<style scoped>
/* Opsional: smooth hover shadow untuk kartu */

</style>
