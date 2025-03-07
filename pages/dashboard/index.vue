<template>
    <div>
      <h1 class="text-2xl font-semibold text-gray-900">Admin Dashboard</h1>
      
      <div class="mt-6 grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3">
        <!-- Total Donations Card -->
        <div class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <div class="flex-shrink-0 bg-indigo-500 rounded-md p-3">
                <svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              </div>
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">Total Donations</dt>
                  <dd>
                    <div class="text-lg font-medium text-gray-900">$23,456.78</div>
                  </dd>
                </dl>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-5 py-3">
            <div class="text-sm">
              <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500">View all</a>
            </div>
          </div>
        </div>
  
        <!-- Pending Transactions Card -->
        <div class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <div class="flex-shrink-0 bg-yellow-500 rounded-md p-3">
                <svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              </div>
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">Pending Transactions</dt>
                  <dd>
                    <div class="text-lg font-medium text-gray-900">12</div>
                  </dd>
                </dl>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-5 py-3">
            <div class="text-sm">
              <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500">Process now</a>
            </div>
          </div>
        </div>
  
        <!-- Member Growth Card -->
        <div class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <div class="flex-shrink-0 bg-green-500 rounded-md p-3">
                <svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
                </svg>
              </div>
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">Member Growth</dt>
                  <dd>
                    <div class="text-lg font-medium text-gray-900">+24% <span class="text-sm text-gray-500">vs last month</span></div>
                  </dd>
                </dl>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-5 py-3">
            <div class="text-sm">
              <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500">View analytics</a>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Monthly Financial Summary Chart -->
      <div class="mt-8">
        <div class="bg-white shadow rounded-lg">
          <div class="px-5 py-4 border-b border-gray-200">
            <h3 class="text-lg leading-6 font-medium text-gray-900">Monthly Financial Summary</h3>
          </div>
          <div class="p-5">
            <div class="h-80">
              <canvas ref="financialChart"></canvas>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Quick Actions and Notifications -->
      <div class="mt-8 grid grid-cols-1 gap-5 sm:grid-cols-2">
        <!-- Quick Actions -->
        <div class="bg-white shadow rounded-lg">
          <div class="px-5 py-4 border-b border-gray-200">
            <h3 class="text-lg leading-6 font-medium text-gray-900">Quick Actions</h3>
          </div>
          <div class="p-5">
            <div class="grid grid-cols-2 gap-4">
              <button class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                <svg class="mr-2 -ml-1 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                </svg>
                Add Event
              </button>
              <button class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                <svg class="mr-2 -ml-1 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 9V7a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2m2 4h10a2 2 0 002-2v-6a2 2 0 00-2-2H9a2 2 0 00-2 2v6a2 2 0 002 2zm7-5a2 2 0 11-4 0 2 2 0 014 0z" />
                </svg>
                Create Payment Link
              </button>
              <button class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                <svg class="mr-2 -ml-1 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                </svg>
                View Reports
              </button>
              <button class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                <svg class="mr-2 -ml-1 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z" />
                </svg>
                Add Member
              </button>
            </div>
          </div>
        </div>
  
        <!-- Notifications Panel -->
        <div class="bg-white shadow rounded-lg">
          <div class="px-5 py-4 border-b border-gray-200">
            <h3 class="text-lg leading-6 font-medium text-gray-900">Notifications</h3>
          </div>
          <div class="p-5">
            <ul class="divide-y divide-gray-200">
              <li class="py-4">
                <div class="flex space-x-3">
                  <div class="flex-shrink-0">
                    <svg class="h-6 w-6 text-green-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                  </div>
                  <div class="min-w-0 flex-1">
                    <p class="text-sm font-medium text-gray-900">New donation received</p>
                    <p class="text-sm text-gray-500">$500 from John Doe</p>
                  </div>
                  <div class="flex-shrink-0 whitespace-nowrap text-sm text-gray-500">
                    10 minutes ago
                  </div>
                </div>
              </li>
              <li class="py-4">
                <div class="flex space-x-3">
                  <div class="flex-shrink-0">
                    <svg class="h-6 w-6 text-blue-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                    </svg>
                  </div>
                  <div class="min-w-0 flex-1">
                    <p class="text-sm font-medium text-gray-900">Upcoming event reminder</p>
                    <p class="text-sm text-gray-500">Sunday Service at 10:00 AM</p>
                  </div>
                  <div class="flex-shrink-0 whitespace-nowrap text-sm text-gray-500">
                    1 hour ago
                  </div>
                </div>
              </li>
              <li class="py-4">
                <div class="flex space-x-3">
                  <div class="flex-shrink-0">
                    <svg class="h-6 w-6 text-yellow-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                    </svg>
                  </div>
                  <div class="min-w-0 flex-1">
                    <p class="text-sm font-medium text-gray-900">Payment processing issue</p>
                    <p class="text-sm text-gray-500">Failed transaction for monthly tithe</p>
                  </div>
                  <div class="flex-shrink-0 whitespace-nowrap text-sm text-gray-500">
                    3 hours ago
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  
  const financialChart = ref<HTMLCanvasElement | null>(null)
  
  definePageMeta({
    layout: 'dashboard'
  })
  
  onMounted(() => {
    if (typeof window !== 'undefined' && financialChart.value) {
      // In a real application, you would import and use Chart.js
      // This is a placeholder for the chart rendering
      console.log('Chart would be rendered here')
      
      // Example of how you would use Chart.js:
      // import Chart from 'chart.js/auto'
      // 
      // new Chart(financialChart.value, {
      //   type: 'bar',
      //   data: {
      //     labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
      //     datasets: [
      //       {
      //         label: 'Income',
      //         data: [12000, 19000, 15000, 17000, 22000, 24000],
      //         backgroundColor: 'rgba(79, 70, 229, 0.8)',
      //       },
      //       {
      //         label: 'Expenses',
      //         data: [8000, 12000, 10000, 9000, 14000, 16000],
      //         backgroundColor: 'rgba(239, 68, 68, 0.8)',
      //       }
      //     ]
      //   },
      //   options: {
      //     responsive: true,
      //     maintainAspectRatio: false,
      //   }
      // })
    }
  })
  </script>