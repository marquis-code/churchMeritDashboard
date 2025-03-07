<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold">Workforce Training & Certification</h1>
      
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <!-- Online Training Modules -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-xl font-semibold mb-4">Online Training Modules</h2>
          <ul class="space-y-2">
            <li v-for="module in trainingModules" :key="module.id" class="flex items-center">
              <icon-book-open class="mr-2 text-blue-500" />
              <span>{{ module.title }}</span>
            </li>
          </ul>
          <button @click="startNewModule" class="mt-4 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition-colors">
            Start New Module
          </button>
        </div>
  
        <!-- Certification Progress -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-xl font-semibold mb-4">Certification Progress</h2>
          <div v-for="cert in certifications" :key="cert.id" class="mb-4">
            <div class="flex justify-between items-center mb-2">
              <span>{{ cert.name }}</span>
              <span class="text-sm text-gray-600">{{ cert.progress }}%</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-2.5">
              <div class="bg-green-500 h-2.5 rounded-full" :style="{ width: `${cert.progress}%` }"></div>
            </div>
          </div>
        </div>
  
        <!-- Training Progress Tracking -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-xl font-semibold mb-4">Training Progress</h2>
          <canvas ref="progressChart"></canvas>
        </div>
      </div>
  
      <!-- Upcoming Training Sessions -->
      <div class="mt-8 bg-white rounded-lg shadow-md p-6">
        <h2 class="text-xl font-semibold mb-4">Upcoming Training Sessions</h2>
        <table class="w-full">
          <thead>
            <tr class="bg-gray-100">
              <th class="p-2 text-left">Session</th>
              <th class="p-2 text-left">Date</th>
              <th class="p-2 text-left">Time</th>
              <th class="p-2 text-left">Instructor</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="session in upcomingSessions" :key="session.id" class="border-b">
              <td class="p-2">{{ session.title }}</td>
              <td class="p-2">{{ session.date }}</td>
              <td class="p-2">{{ session.time }}</td>
              <td class="p-2">{{ session.instructor }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import { Chart, registerables } from 'chart.js'
  // import { IconBookOpen } from '@iconify/vue'
  
  Chart.register(...registerables)

  definePageMeta({
    layout: 'dashboard'
  })
  
  const progressChart = ref<HTMLCanvasElement | null>(null)
  
  const trainingModules = ref([
    { id: 1, title: 'Church Leadership Basics' },
    { id: 2, title: 'Effective Communication' },
    { id: 3, title: 'Financial Stewardship' },
    { id: 4, title: 'Community Outreach' },
  ])
  
  const certifications = ref([
    { id: 1, name: 'Church Administration', progress: 75 },
    { id: 2, name: 'Youth Ministry', progress: 40 },
    { id: 3, name: 'Pastoral Care', progress: 90 },
  ])
  
  const upcomingSessions = ref([
    { id: 1, title: 'Conflict Resolution', date: '2025-03-15', time: '10:00 AM', instructor: 'Dr. Smith' },
    { id: 2, title: 'Digital Ministry', date: '2025-03-22', time: '2:00 PM', instructor: 'Pastor Johnson' },
    { id: 3, title: 'Volunteer Management', date: '2025-03-29', time: '11:00 AM', instructor: 'Mrs. Davis' },
  ])
  
  const startNewModule = () => {
    // Logic to start a new training module
    alert('Starting a new training module')
  }
  
  onMounted(() => {
    if (progressChart.value) {
      new Chart(progressChart.value, {
        type: 'line',
        data: {
          labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
          datasets: [{
            label: 'Training Hours',
            data: [5, 10, 15, 20, 25, 30],
            borderColor: 'rgb(75, 192, 192)',
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      })
    }
  })
  </script>