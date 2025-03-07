<!-- pages/finance-dashboard/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold">Finance Dashboard</h1>
  
      <!-- Financial Overview -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <div v-for="metric in financialMetrics" :key="metric.name" 
             class="bg-white rounded-lg shadow-md p-6">
          <h3 class="text-lg font-semibold mb-2">{{ metric.name }}</h3>
          <p class="text-3xl font-bold" :class="metric.color">
            {{ metric.prefix }}{{ metric.value.toLocaleString() }}{{ metric.suffix }}
          </p>
          <p class="text-sm text-gray-500 mt-2">
            <span :class="metric.trend === 'up' ? 'text-green-500' : 'text-red-500'">
              {{ metric.trend === 'up' ? '↑' : '↓' }} {{ metric.changePercentage }}%
            </span>
            vs last month
          </p>
        </div>
      </div>
  
      <!-- Donation Trends Chart -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Donation Trends</h2>
        <canvas ref="donationTrendsChart"></canvas>
      </div>
  
      <!-- Remittance Reports -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Remittance Reports</h2>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-gray-50">
                <th class="px-4 py-2 text-left">Date</th>
                <th class="px-4 py-2 text-left">Description</th>
                <th class="px-4 py-2 text-left">Amount</th>
                <th class="px-4 py-2 text-left">Status</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="remittance in remittances" :key="remittance.id" class="border-t">
                <td class="px-4 py-2">{{ remittance.date }}</td>
                <td class="px-4 py-2">{{ remittance.description }}</td>
                <td class="px-4 py-2">${{ remittance.amount.toFixed(2) }}</td>
                <td class="px-4 py-2">
                  <span :class="{
                    'bg-green-100 text-green-800': remittance.status === 'Completed',
                    'bg-yellow-100 text-yellow-800': remittance.status === 'Pending',
                    'bg-red-100 text-red-800': remittance.status === 'Failed'
                  }" class="px-2 py-1 rounded-full text-xs">
                    {{ remittance.status }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <!-- Outstanding Payments & Deductions -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Outstanding Payments & Deductions</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <h3 class="text-lg font-medium mb-2">Payments Due</h3>
            <ul class="space-y-2">
              <li v-for="payment in outstandingPayments" :key="payment.id" 
                  class="flex justify-between items-center">
                <span>{{ payment.description }}</span>
                <span class="font-semibold">${{ payment.amount.toFixed(2) }}</span>
              </li>
            </ul>
          </div>
          <div>
            <h3 class="text-lg font-medium mb-2">Upcoming Deductions</h3>
            <ul class="space-y-2">
              <li v-for="deduction in upcomingDeductions" :key="deduction.id" 
                  class="flex justify-between items-center">
                <span>{{ deduction.description }}</span>
                <span class="font-semibold">${{ deduction.amount.toFixed(2) }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
  
      <!-- Fund Transfers Summary -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-2xl font-semibold mb-4">Fund Transfers Summary</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div v-for="transfer in fundTransfers" :key="transfer.id" 
               class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">{{ transfer.description }}</h3>
            <p class="text-2xl font-bold mb-2">${{ transfer.amount.toFixed(2) }}</p>
            <p class="text-sm text-gray-500">From: {{ transfer.from }}</p>
            <p class="text-sm text-gray-500">To: {{ transfer.to }}</p>
            <p class="text-sm text-gray-500">Date: {{ transfer.date }}</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import Chart from 'chart.js/auto'
  
  const donationTrendsChart = ref<HTMLCanvasElement | null>(null)
  
  const financialMetrics = ref([
    { name: 'Total Donations', value: 150000, prefix: '$', suffix: '', color: 'text-green-600', trend: 'up', changePercentage: 12 },
    { name: 'Expenses', value: 80000, prefix: '$', suffix: '', color: 'text-red-600', trend: 'down', changePercentage: 5 },
    { name: 'Net Income', value: 70000, prefix: '$', suffix: '', color: 'text-blue-600', trend: 'up', changePercentage: 8 },
    { name: 'Donor Growth', value: 15, prefix: '', suffix: '%', color: 'text-purple-600', trend: 'up', changePercentage: 3 },
  ])
  
  const remittances = ref([
    { id: 1, date: '2025-03-01', description: 'Monthly Tithe Remittance', amount: 5000, status: 'Completed' },
    { id: 2, date: '2025-03-15', description: 'Mission Fund Transfer', amount: 2000, status: 'Pending' },
    // Add more remittances...
  ])
  
  const outstandingPayments = ref([
    { id: 1, description: 'Utility Bill', amount: 500 },
    { id: 2, description: 'Staff Salaries', amount: 10000 },
    // Add more outstanding payments...
  ])
  
  const upcomingDeductions = ref([
    { id: 1, description: 'Insurance Premium', amount: 1000 },
    { id: 2, description: 'Loan Repayment', amount: 2000 },
    // Add more upcoming deductions...
  ])
  
  const fundTransfers = ref([
    { id: 1, description: 'Building Fund Transfer', amount: 25000, from: 'General Fund', to: 'Building Fund', date: '2025-03-10' },
    { id: 2, description: 'Mission Trip Allocation', amount: 5000, from: 'Missions Fund', to: 'Trip Expenses', date: '2025-03-20' },
    // Add more fund transfers...
  ])

  definePageMeta({
    layout: 'dashboard'
  })
  
  
  onMounted(() => {
    if (donationTrendsChart.value) {
      new Chart(donationTrendsChart.value, {
        type: 'line',
        data: {
          labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
          datasets: [
            {
              label: 'Tithes',
              data: [12000, 15000, 18000, 16000, 20000, 22000],
              borderColor: 'rgb(59, 130, 246)',
              tension: 0.1
            },
            {
              label: 'Offerings',
              data: [5000, 6000, 7000, 6500, 8000, 9000],
              borderColor: 'rgb(16, 185, 129)',
              tension: 0.1
            },
            {
              label: 'Special Donations',
              data: [3000, 2000, 5000, 4000, 6000, 7000],
              borderColor: 'rgb(249, 115, 22)',
              tension: 0.1
            }
          ]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top',
            },
            title: {
              display: true,
              text: 'Donation Trends'
            }
          }
        }
      })
    }
  })
  </script>