<template>
  <div class="flex h-screen" style="background-color: #f8fafc; background-image: radial-gradient(circle at top right, rgba(34, 197, 94, 0.4), transparent 50%);">
    <Sidebar />
    <main class="flex-1 p-6 overflow-y-auto">
      <div class="bg-white/95 backdrop-blur-md rounded-2xl shadow-xl w-full p-6 min-h-full">
        <!-- Breadcrumb -->
        <div class="flex items-center gap-4 mb-6">
          <div class="bg-white p-2 rounded-md shadow">
            <svg class="w-6 h-6 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-7-4h6"></path></svg>
          </div>
          <div class="flex items-center text-xl font-bold text-gray-800">
            <router-link to="/admin/classroom" class="hover:underline">Classroom</router-link>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mx-2 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.5">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7" />
            </svg>
            <span>{{ classroomName }}</span>
          </div>
        </div>

        <!-- Tabs -->
        <div class="border-b border-gray-200">
          <nav class="-mb-px flex space-x-8" aria-label="Tabs">
            <button @click="activeTab = 'mataPelajaran'" :class="[activeTab === 'mataPelajaran' ? 'border-green-500 text-green-600' : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300', 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm']">
              Mata Pelajaran
            </button>
            <button @click="activeTab = 'tugas'" :class="[activeTab === 'tugas' ? 'border-green-500 text-green-600' : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300', 'whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm']">
              Tugas
            </button>
          </nav>
        </div>

        <!-- Mata Pelajaran View -->
        <div v-if="activeTab === 'mataPelajaran'" class="mt-6">
          <!-- Header: Title and Button -->
          <div class="flex flex-wrap gap-4 justify-between items-center mb-6">
            <h1 class="text-2xl font-bold text-gray-800">Mata Pelajaran</h1>
            <button class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-lg flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
              </svg>
              Tambah Mata Pelajaran
            </button>
          </div>

          <!-- Search and Filters -->
          <div class="flex flex-wrap gap-4 justify-between items-center mb-6">
            <div class="relative w-full sm:w-auto sm:max-w-xs">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg class="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                </svg>
              </div>
              <input type="text" placeholder="Cari mata pelajaran" class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg text-sm focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500">
            </div>
            <div class="flex space-x-4">
              <select class="border border-gray-300 rounded-lg text-sm px-4 py-2 focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500 bg-white">
                <option>Semua Kelas</option>
              </select>
              <select class="border border-gray-300 rounded-lg text-sm px-4 py-2 focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500 bg-white">
                <option>Semua Status</option>
              </select>
            </div>
          </div>

          <!-- Subject Cards Grid -->
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="subject in subjects" :key="subject.name" @click="goToTheme(subject)" class="bg-white rounded-2xl p-5 border border-gray-100 hover:shadow-lg transition-shadow duration-300 cursor-pointer">
              <div class="flex justify-between items-start">
                <div class="flex items-center space-x-3">
                  <div :class="`bg-${subject.iconColor}-100 rounded-lg p-3 flex items-center justify-center`">
                    <component :is="subject.icon" :class="`h-6 w-6 text-${subject.iconColor}-600`" />
                  </div>
                  <div>
                    <h3 class="font-bold text-gray-800">{{ subject.name }}</h3>
                    <p class="text-xs text-gray-500">{{ subject.class }}</p>
                  </div>
                </div>
                <div class="flex items-center space-x-1 text-gray-400">
                  <button class="hover:text-gray-600 p-1 rounded-full hover:bg-gray-100">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.5L15.232 5.232z"></path></svg>
                  </button>
                  <button class="hover:text-gray-600 p-1 rounded-full hover:bg-gray-100">
                     <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z"></path></svg>
                  </button>
                </div>
              </div>
              <div class="flex justify-between text-sm text-gray-600 mt-5">
                <div class="flex items-center">
                  <svg class="w-4 h-4 mr-2 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.246 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>
                  <span>{{ subject.topics }} Topik</span>
                </div>
                <div class="flex items-center">
                  <svg class="w-4 h-4 mr-2 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002 2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" /></svg>
                  <span>{{ subject.assignments }} Tugas Aktif</span>
                </div>
              </div>
              <div class="flex justify-between items-center mt-4 pt-4 border-t border-gray-100">
                <div class="flex items-center">
                  <img :src="subject.teacher_avatar" :alt="subject.teacher" class="w-8 h-8 rounded-full mr-3 object-cover">
                  <span class="text-sm text-gray-700">{{ subject.teacher }}</span>
                </div>
                <span v-if="subject.status === 'Aktif'" class="text-xs font-semibold bg-green-100 text-green-700 px-3 py-1 rounded-full">Aktif</span>
                <span v-else class="text-xs font-semibold bg-yellow-100 text-yellow-800 px-3 py-1 rounded-full">Arsip</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Tugas View -->
        <div v-if="activeTab === 'tugas'" class="mt-6">
           <!-- Header: Title and Button -->
           <div class="flex flex-wrap gap-4 justify-between items-center mb-6">
            <h1 class="text-2xl font-bold text-gray-800">Daftar Tugas</h1>
            <button class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-lg flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
              </svg>
              Tambah Tugas
            </button>
          </div>

          <!-- Search and Filters -->
          <div class="flex flex-wrap gap-4 justify-between items-center mb-6">
            <div class="relative w-full sm:w-auto sm:max-w-xs">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg class="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                </svg>
              </div>
              <input type="text" placeholder="Cari tugas" class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg text-sm focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500">
            </div>
            <div class="flex space-x-4">
              <select class="border border-gray-300 rounded-lg text-sm px-4 py-2 focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500 bg-white">
                <option>Semua Kelas</option>
              </select>
              <select class="border border-gray-300 rounded-lg text-sm px-4 py-2 focus:outline-none focus:ring-1 focus:ring-green-500 focus:border-green-500 bg-white">
                <option>Semua Status</option>
              </select>
            </div>
          </div>
          
          <!-- Assignments Table -->
          <div class="overflow-x-auto">
            <table class="min-w-full text-sm text-left">
              <thead class="text-xs text-gray-500 uppercase bg-gray-50">
                <tr>
                  <th scope="col" class="py-3 px-6">Judul Tugas</th>
                  <th scope="col" class="py-3 px-6">Mata Pelajaran</th>
                  <th scope="col" class="py-3 px-6">Deadline</th>
                  <th scope="col" class="py-3 px-6">Status</th>
                  <th scope="col" class="py-3 px-6">Submission</th>
                  <th scope="col" class="py-3 px-6">Aksi</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="assignment in assignments" :key="assignment.id" class="bg-white border-b hover:bg-gray-50">
                  <td class="py-4 px-6">
                    <div class="font-medium text-gray-900">{{ assignment.title }}</div>
                    <div class="text-xs text-gray-500">{{ assignment.subtitle }}</div>
                  </td>
                  <td class="py-4 px-6">
                    <div>{{ assignment.subject }}</div>
                    <div class="text-xs text-gray-500">{{ assignment.class }}</div>
                  </td>
                  <td class="py-4 px-6">
                    <div>{{ assignment.deadline_date }}</div>
                    <div class="text-xs text-gray-500">{{ assignment.deadline_time }}</div>
                  </td>
                  <td class="py-4 px-6">
                    <span :class="[assignment.status === 'Aktif' ? 'bg-yellow-100 text-yellow-800' : 'bg-green-100 text-green-800', 'text-xs font-medium mr-2 px-2.5 py-0.5 rounded-full']">{{ assignment.status }}</span>
                  </td>
                  <td class="py-4 px-6">{{ assignment.submission }}</td>
                  <td class="py-4 px-6">
                    <div class="flex items-center space-x-2">
                       <button class="text-gray-400 hover:text-blue-600">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"></path></svg>
                      </button>
                      <button class="text-gray-400 hover:text-green-600">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"></path></svg>
                      </button>
                       <button class="text-gray-400 hover:text-red-600">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Pagination -->
          <div class="flex justify-between items-center mt-6 text-sm text-gray-600">
            <div>Menampilkan 1 sampai 5 dari 12 tugas</div>
            <div class="flex items-center">
              <button class="p-2 hover:bg-gray-100 rounded-md disabled:text-gray-300">&lt;</button>
              <button class="px-4 py-2 bg-green-500 text-white rounded-md mx-1">1</button>
              <button class="px-4 py-2 hover:bg-gray-100 rounded-md mx-1">2</button>
              <button class="px-4 py-2 hover:bg-gray-100 rounded-md mx-1">3</button>
              <button class="p-2 hover:bg-gray-100 rounded-md">&gt;</button>
            </div>
          </div>
        </div>

      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, markRaw, h, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { useClassroomStore } from '@/stores/classroom'
import { useSubjectStore } from '@/stores/subject'
import Sidebar from '@/components/admin/Sidebar.vue'
import { BeakerIcon, GlobeAltIcon, AcademicCapIcon, ClockIcon } from '@heroicons/vue/24/outline'

const activeTab = ref('mataPelajaran') // 'mataPelajaran' or 'tugas'

const router = useRouter()
const classroomStore = useClassroomStore()
const subjectStore = useSubjectStore()

const classroomName = computed(() => classroomStore.currentClassroomName)

onMounted(() => {
  if (!classroomName.value) {
    router.push('/admin/classroom')
  }
})

function goToTheme(subject) {
  subjectStore.setSubject(subject)
  router.push({ name: 'admin-subject-theme' })
}

// These classes are dynamically generated, so we need to include them here
// to make sure Tailwind's JIT compiler picks them up.
const safelist = [
  'bg-blue-100', 'text-blue-600',
  'bg-red-100', 'text-red-600',
  'bg-yellow-100', 'text-yellow-600', 'text-yellow-800',
  'bg-green-100', 'text-green-600', 'text-green-800',
  'bg-indigo-100', 'text-indigo-600',
  'bg-pink-100', 'text-pink-600',
  'bg-blue-500', 'text-blue-600',
  'bg-red-500', 'text-red-600',
];

const subjects = ref([
  {
    name: 'Matematika',
    class: 'Kelas 11 - Semester Genap',
    topics: 12,
    assignments: 5,
    teacher: 'Ibu Siti Nurhayati',
    teacher_avatar: `https://ui-avatars.com/api/?name=Siti+Nurhayati&background=random&color=fff`,
    status: 'Aktif',
    iconColor: 'blue',
    icon: markRaw({ render: () => h('div', { class: 'text-blue-600 font-bold text-xl' }, 'O') })
  },
  {
    name: 'Kimia',
    class: 'Kelas 10 - Semester Ganjil',
    topics: 8,
    assignments: 3,
    teacher: 'Bapak Agus Santoso',
    teacher_avatar: `https://ui-avatars.com/api/?name=Agus+Santoso&background=random&color=fff`,
    status: 'Aktif',
    iconColor: 'red',
    icon: markRaw(BeakerIcon)
  },
  {
    name: 'Bahasa Indonesia',
    class: 'Kelas 12 - Semester Genap',
    topics: 10,
    assignments: 2,
    teacher: 'Ibu Dewi Lestari',
    teacher_avatar: `https://ui-avatars.com/api/?name=Dewi+Lestari&background=random&color=fff`,
    status: 'Aktif',
    iconColor: 'yellow',
    icon: markRaw(AcademicCapIcon)
  },
  {
    name: 'Geografi',
    class: 'Kelas 11 - Semester Ganjil',
    topics: 9,
    assignments: 4,
    teacher: 'Bapak Budi Santoso',
    teacher_avatar: `https://ui-avatars.com/api/?name=Budi+Santoso&background=random&color=fff`,
    status: 'Aktif',
    iconColor: 'green',
    icon: markRaw(GlobeAltIcon)
  },
  {
    name: 'Bahasa Inggris',
    class: 'Kelas 10 - Semester Genap',
    topics: 11,
    assignments: 6,
    teacher: 'Ibu Ratna Wijaya',
    teacher_avatar: `https://ui-avatars.com/api/?name=Ratna+Wijaya&background=random&color=fff`,
    status: 'Aktif',
    iconColor: 'indigo',
    icon: markRaw({ render: () => h('span', { class: 'font-bold text-indigo-600 text-lg' }, 'En') })
  },
  {
    name: 'Sejarah',
    class: 'Kelas 12 - Semester Ganjil',
    topics: 7,
    assignments: 1,
    teacher: 'Bapak Hendra Wijaya',
    teacher_avatar: `https://ui-avatars.com/api/?name=Hendra+Wijaya&background=random&color=fff`,
    status: 'Arsip',
    iconColor: 'pink',
    icon: markRaw(ClockIcon)
  }
])

const assignments = ref([
  { id: 1, title: 'Latihan Persamaan Kuadrat', subtitle: 'Persamaan Kuadrat', subject: 'Matematika', class: 'Kelas 11', deadline_date: '30 Mei 2025', deadline_time: '23:59', status: 'Aktif', submission: '15/30' },
  { id: 2, title: 'Quiz Fungsi Trigonometri', subtitle: 'Fungsi Trigonometri', subject: 'Matematika', class: 'Kelas 11', deadline_date: '25 Mei 2025', deadline_time: '23:59', status: 'Aktif', submission: '20/30' },
  { id: 3, title: 'Laporan Praktikum Reaksi Kimia', subtitle: 'Reaksi Kimia', subject: 'Kimia', class: 'Kelas 10', deadline_date: '28 Mei 2025', deadline_time: '23:59', status: 'Aktif', submission: '12/25' },
  { id: 4, title: 'Analisis Puisi Kontemporer', subtitle: 'Puisi Kontemporer', subject: 'Bahasa Indonesia', class: 'Kelas 12', deadline_date: '22 Mei 2025', deadline_time: '23:59', status: 'Selesai', submission: '28/28' },
  { id: 5, title: 'Latihan Limit Fungsi', subtitle: 'Limit Fungsi', subject: 'Matematika', class: 'Kelas 11', deadline_date: '20 Mei 2025', deadline_time: '23:59', status: 'Selesai', submission: '18/30' },
])
</script>