<template>
  <div class="max-w-6xl mx-auto px-4 py-16 text-center" id="portofolioProject">
    <h2 class="text-3xl font-bold text-gray-800 mb-6">Portofolio Project</h2>

    <!-- SEARCH BAR -->
    <input
      type="text"
      v-model="search"
      placeholder="Cari projek..."
      class="w-full max-w-md mx-auto mb-6 px-4 py-2 border border-gray-300 rounded shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
    />

    <!-- SORT & FILTER -->
    <div class="max-w-md mx-auto mb-6 flex flex-col md:flex-col justify-center gap-4 items-center">
      <!-- Filter kategori -->
      <div class="flex flex-wrap justify-center gap-3">
        <button
          v-for="(tag, index) in allTags"
          :key="index"
          @click="(e) => { selectTag(tag); animateButton(e) }"
          :class="[
            'px-4 py-1 rounded-full border transition',
            selectedTag === tag
              ? 'bg-blue-600 text-white border-blue-600'
              : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-100'
          ]"
        >
          {{ tag }}
        </button>
      </div>

      <!-- Sort -->
   <label class="flex items-center gap-2">
        <span class="text-gray-700 font-semibold">Sort by:</span>
        <select
          v-model="sortOrder"
          class="border rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
          @change="animateButton($event)"
        >
          <option value="latest">Latest</option>
          <option value="oldest">Oldest</option>
          <option value="name-asc">Name A-Z</option>
          <option value="name-desc">Name Z-A</option>
        </select>
      </label>
    </div>

    <!-- DAFTAR PROJECT -->
    <div
      ref="gridRef"
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10 cursor-pointer"
    >
      <div
        v-for="(project, index) in paginatedProjects"
        :key="project.name"
        class="rounded-xl shadow-lg overflow-hidden bg-white hover:shadow-2xl transition duration-300 group"
        @click="openProject(project)"
      >
        <img
          :src="project.image"
          alt="Project"
          loading="lazy"
          class="w-full h-48 object-cover group-hover:scale-105 transition duration-300"
        />
        <div class="p-4 text-left">
          <h3 class="text-xl font-semibold text-blue-600">{{ project.name }}</h3>
          <p class="text-sm text-gray-600 mt-1 truncate">{{ project.desc }}</p>
          <div class="text-xs mt-2 text-gray-400 italic">
            {{ project.tech.join(', ') }}
          </div>
          <div class="text-xs mt-1 text-gray-500">Tanggal: {{ project.date }}</div>
          <div class="flex gap-4 justify-start mt-4">
            <a
              v-if="project.demo"
              :href="project.demo"
              target="_blank"
              @click.stop
              class="text-sm text-white bg-blue-600 px-3 py-1 rounded hover:bg-blue-700"
            >
              Demo
            </a>
            <a
              v-if="project.github"
              :href="project.github"
              target="_blank"
              @click.stop
              class="text-sm text-blue-600 border border-blue-600 px-3 py-1 rounded hover:bg-blue-600 hover:text-white"
            >
              GitHub
            </a>
          </div>
        </div>
      </div>
    </div>

    <!-- PAGINATION BUTTONS -->
    <div class="flex justify-center gap-3 mt-10">
      <button
        v-for="page in totalPages"
        :key="page"
        @click="(e) => { currentPage = page; animateButton(e) }"
        :class="[
          'px-4 py-1 rounded border transition',
          currentPage === page
            ? 'bg-blue-600 text-white border-blue-600'
            : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-100'
        ]"
      >
        {{ page }}
      </button>
    </div>

    <!-- MODAL DETAIL PROJECT -->
    <div
      v-if="selectedProject"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      @click.self="selectedProject = null"
    >
      <div
        class="bg-white rounded-lg max-w-lg w-full p-6 relative"
        @click.stop
      >
        <button
          class="absolute top-2 right-2 text-gray-600 hover:text-gray-900 text-2xl font-bold"
          @click="selectedProject = null"
          aria-label="Close modal"
        >
          &times;
        </button>
        <img
          :src="selectedProject.image"
          alt="Project Image"
          class="w-full h-48 object-cover rounded mb-4"
        />
        <h3 class="text-2xl font-semibold text-blue-600 mb-2">
          {{ selectedProject.name }}
        </h3>
        <p class="mb-2 text-start">{{ selectedProject.desc }}</p>
        <div class="mb-2 text-gray-600 italic">
          Tech: {{ selectedProject.tech.join(', ') }}
        </div>
        <div class="mb-4">Tanggal: {{ selectedProject.date }}</div>
        <div class="flex gap-4">
          <a
            v-if="selectedProject.demo"
            :href="selectedProject.demo"
            target="_blank"
            class="text-sm text-white bg-blue-600 px-3 py-1 rounded hover:bg-blue-700"
          >
            Demo
          </a>
          <a
            v-if="selectedProject.github"
            :href="selectedProject.github"
            target="_blank"
            class="text-sm text-blue-600 border border-blue-600 px-3 py-1 rounded hover:bg-blue-600 hover:text-white"
          >
            GitHub
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, nextTick, onMounted } from 'vue'
import gsap from 'gsap'

interface Project {
  name: string
  desc: string
  tech: string[]
  image: string
  demo?: string
  github?: string
  date: string
}

const projects = ref([
  {
    name: 'Rantai Tani',
    desc: 'Platform web terintegrasi ini dirancang sebagai solusi komprehensif untuk bisnis yang membutuhkan manajemen stok produk secara akurat, pengelolaan hubungan investor yang transparan, dan pemantauan transaksi setiap mitra secara efisien. Dengan fitur terpusat untuk pelacakan inventaris real-time, pelaporan investor, dan analisis performa mitra, sistem ini bertujuan meningkatkan efisiensi operasional, memberikan visibilitas data yang menyeluruh, serta mendukung pengambilan keputusan strategis berbasis data bagi seluruh pemangku kepentingan.',
    tech: ['Laravel', 'Mysql', 'Bootstrap'],
    image: '/projects/rantaitani.png',
    // demo: 'https://rantai-tani.com',
    // github: 'https://github.com/yourname/tokosayur',
    date: '2025-10-15'
  },
  {
    name: 'Demangan Farm',
    desc: 'Dashboard monitoring real-time ini menyajikan pandangan komprehensif terhadap operasional pertanian Anda dalam satu tampilan terpusat, menampilkan secara langsung data krusial seperti suhu dan kelembaban lingkungan, volume air, status ketersediaan pakan atau nutrisi esensial (makanan), serta status on/off berbagai sensor penting yang tersebar di seluruh area pertanian atau peternakan. Dengan visualisasi data yang akurat dan terkini, platform ini memberdayakan para petani atau pengelola untuk mengambil keputusan secara cepat dan proaktif, mengoptimalkan penggunaan sumber daya, menjaga kondisi lingkungan ideal bagi pertumbuhan tanaman maupun kesehatan ternak, dan pada akhirnya meningkatkan efisiensi serta produktivitas usaha tani secara signifikan.',
    tech: ['Laravel', 'Firebase','Bootstrap'],
    image: '/projects/demanganfarm.png',
    demo: '',
    // github: 'https://github.com/yourname/absensi-qr',
    date: '2023-07-22'
  },
  {
    name: 'Absensi Mobile by location',
    desc: 'Sistem absensi modern ini secara akurat mencatat kehadiran dengan memadukan verifikasi lokasi geografis (GPS) untuk memastikan pengguna berada di tempat yang ditentukan, dan teknologi pengenalan wajah untuk mengonfirmasi identitas individu, sehingga mencegah kecurangan dan meningkatkan efisiensi.',
    tech: ['Kotlin', 'Firebase'],
    image: '/projects/absensi.jpg',
    demo: '',
    github: '',
    date: '2023-09-10'
  },
  // {
  //   name: 'Republic Bibit',
  //   desc: 'Website Penjualan macam macam bibit tumbuhan dengan pembayaran payment gateway midtrans dan buah buahan serta dashboard untuk kelola stok dan penjualan.',
  //   tech: ['Vue', 'GSAP', 'Tailwind'],
  //   image: '/projects/portfolio.jpg',
  //   demo: '',
  //   github: '',
  //   date: '2023-06-05'
  // },
  {
    name: 'Republik Bibit',
    desc: 'Website Penjualan macam macam bibit tumbuhan dengan pembayaran payment gateway midtrans dan buah buahan serta dashboard untuk kelola stok dan penjualan.',
    tech: ['Laravel','Mysql', 'Bootstrap'],
    image: '/projects/republikbibit.png',
    demo: '',
    github: '',
    date: '2023-08-12'
  }
])
const selectedTag = ref<string>('All')
const search = ref<string>('')
const sortOrder = ref<string>('latest')
const gridRef = ref<HTMLElement | null>(null)
const currentPage = ref(1)
const perPage = 3

const allTags = computed(() => {
  const tags = new Set<string>(['All'])
  projects.value.forEach(p => p.tech.forEach(t => tags.add(t)))
  return Array.from(tags)
})

const filteredProjects = computed(() =>
  projects.value.filter(p => {
    const matchTag =
      selectedTag.value === 'All' || p.tech.includes(selectedTag.value)
    const matchSearch =
      p.name.toLowerCase().includes(search.value.toLowerCase()) ||
      p.desc.toLowerCase().includes(search.value.toLowerCase())
    return matchTag && matchSearch
  })
)

const sortedProjects = computed(() => {
  const arr = [...filteredProjects.value]
  switch (sortOrder.value) {
    case 'latest':
      return arr.sort((a, b) => (a.date < b.date ? 1 : -1))
    case 'oldest':
      return arr.sort((a, b) => (a.date > b.date ? 1 : -1))
    case 'name-asc':
      return arr.sort((a, b) => a.name.localeCompare(b.name))
    case 'name-desc':
      return arr.sort((a, b) => b.name.localeCompare(a.name))
    default:
      return arr
  }
})

const totalPages = computed(() =>
  Math.ceil(sortedProjects.value.length / perPage)
)

const paginatedProjects = computed(() =>
  sortedProjects.value.slice(
    (currentPage.value - 1) * perPage,
    currentPage.value * perPage
  )
)

function selectTag(tag: string) {
  selectedTag.value = tag
  currentPage.value = 1
}

function animateButton(e: Event) {
  const target = e.target as HTMLElement
  if (target) {
    gsap.fromTo(
      target,
      { scale: 1 },
      { scale: 1.1, duration: 0.1, yoyo: true, repeat: 1 }
    )
  }
}

const selectedProject = ref<Project | null>(null)
function openProject(project: Project) {
  selectedProject.value = project
}

watch([search, selectedTag, sortOrder], () => {
  currentPage.value = 1
  nextTick(() => {
    if (gridRef.value?.children) {
      gsap.fromTo(
        gridRef.value.children,
        { opacity: 0, y: 30 },
        {
          opacity: 1,
          y: 0,
          stagger: 0.1,
          duration: 0.3
        }
      )
    }
  })
})

onMounted(() => {
  if (gridRef.value?.children) {
    gsap.fromTo(
      gridRef.value.children,
      { opacity: 0, y: 30 },
      {
        opacity: 1,
        y: 0,
        stagger: 0.1,
        duration: 0.4
      }
    )
  }
})
</script>


<style scoped>
/* Custom scrollbar for modal if needed */
</style>
