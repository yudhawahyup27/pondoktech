<template>
  <div ref="heroSection" class="min-h-screen flex items-center justify-center bg-white overflow-hidden">
    <div class="w-full max-w-6xl grid grid-cols-1 md:grid-cols-2 gap-8 items-center px-6">

      <!-- Kolom Kiri -->
      <div ref="leftColumn" class="text-center md:text-left space-y-4">
        <h3 ref="title" class="text-blue-500 text-4xl font-bold">Pondok Tech</h3>
        <h4 ref="subtitle" class="font-bold text-xl md:text-2xl">
          Innovation Without Limits. Any Technology, We Are Ready to Build It!
        </h4>
        <p ref="description" class="text-gray-600 text-sm md:text-base">
          From stunning UI/UX design, responsive Web & Mobile development, to cutting-edge IoT solutions – Bring your digital vision to life with us.
        </p>

        <a

          href="https://api.whatsapp.com/send?phone=6285850615979&text=Hallo%20Pondok%20Tech%2C%20saya%20mau%20diskusi%20mengenai%20ide%20system%2Fsoftware%20untuk%20kebutuhan%20digital%20saya"
          target="_blank"
          rel="noopener noreferrer"
          class="inline-flex items-center px-6 py-2.5 hover:animate-bounce bg-blue-500 text-white font-medium text-xs leading-tight rounded shadow-md hover:bg-blue-900 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
        >
          <img
            src="https://img.icons8.com/?size=20&id=62855&format=png&color=FFFFFF"
            alt="WhatsApp icon"
            class="mr-2 h-5 w-5" />
          Let's Make Your Ideas Come True
        </a>
      </div>

      <!-- Kolom Kanan -->
      <div ref="rightColumn" class="flex justify-center">
        <img
          ref="heroImage"
          class="rounded-xl shadow-lg w-full max-w-md"
          src="https://img.freepik.com/free-photo/close-up-young-colleagues-having-meeting_23-2149060239.jpg?semt=ais_hybrid&w=740"
          alt="Hero Image"
        />
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, nextTick } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const heroSection = ref(null)
const leftColumn = ref(null)
const rightColumn = ref(null)
const title = ref(null)
const subtitle = ref(null)
const description = ref(null)
const ctaButton = ref(null)
const heroImage = ref(null)

onMounted(() => {
  nextTick(() => {
    // Initial animation timeline when component loads
    const tl = gsap.timeline()

    // Animate title with typing effect
    tl.from(title.value, {
      opacity: 0,
      x: -100,
      duration: 1,
      ease: 'power3.out'
    })
    .from(subtitle.value, {
      opacity: 0,
      y: 50,
      duration: 0.8,
      ease: 'power2.out'
    }, '-=0.5')
    .from(description.value, {
      opacity: 0,
      y: 30,
      duration: 0.6,
      ease: 'power2.out'
    }, '-=0.4')
    .from(ctaButton.value, {
      opacity: 0,
      scale: 0.8,
      y: 20,
      duration: 0.5,
      ease: 'back.out(1.7)'
    }, '-=0.3')
    .from(heroImage.value, {
      opacity: 0,
      scale: 0.8,
      rotation: 5,
      duration: 1,
      ease: 'power2.out'
    }, '-=0.8')

    // Scroll-triggered animations with better control
    ScrollTrigger.create({
      trigger: heroSection.value,
      start: 'top top',
      end: 'bottom top',
      scrub: 1,
      onUpdate: (self) => {
        const progress = self.progress

        // Parallax background
        gsap.set(heroSection.value, {
          yPercent: -(progress * 50)
        })

        // Text fade and movement - reversible
        gsap.set([title.value, subtitle.value, description.value], {
          opacity: Math.max(0.2, 1 - (progress * 0.8)),
          y: -(progress * 80)
        })

        // Image rotation and scale
        gsap.set(heroImage.value, {
          rotation: progress * 8,
          scale: 1 + (progress * 0.1)
        })
      },
      onLeave: () => {
        // Smooth transition when leaving viewport
        gsap.to([title.value, subtitle.value, description.value], {
          opacity: 0.2,
          duration: 0.3
        })
      },
      onEnterBack: () => {
        // Smooth transition when entering back
        gsap.to([title.value, subtitle.value, description.value], {
          opacity: 1,
          y: 0,
          duration: 0.5,
          ease: 'power2.out'
        })
        gsap.to(heroImage.value, {
          rotation: 0,
          scale: 1,
          duration: 0.5,
          ease: 'power2.out'
        })
      }
    })

    // CTA button floating animation
    gsap.to(ctaButton.value, {
      y: -5,
      duration: 2,
      ease: 'power2.inOut',
      yoyo: true,
      repeat: -1
    })

    // Enhanced hover effects
    const ctaBtn = ctaButton.value
    if (ctaBtn) {
      ctaBtn.addEventListener('mouseenter', () => {
        gsap.to(ctaBtn, {
          scale: 1.05,
          duration: 0.3,
          ease: 'power2.out'
        })
      })

      ctaBtn.addEventListener('mouseleave', () => {
        gsap.to(ctaBtn, {
          scale: 1,
          duration: 0.3,
          ease: 'power2.out'
        })
      })
    }

    // Image hover effect
    const img = heroImage.value
    if (img) {
      img.addEventListener('mouseenter', () => {
        gsap.to(img, {
          scale: 1.05,
          rotation: -2,
          duration: 0.4,
          ease: 'power2.out'
        })
      })

      img.addEventListener('mouseleave', () => {
        gsap.to(img, {
          scale: 1,
          rotation: 0,
          duration: 0.4,
          ease: 'power2.out'
        })
      })
    }
  })
})
</script>
