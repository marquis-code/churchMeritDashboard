<!-- pages/analytics/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-xl font-semibold">Analytics & Reporting</h1>
  
      <!-- Financial Health Dashboard -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4">Financial Health Dashboard</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
          <div v-for="metric in financialMetrics" :key="metric.name" class="bg-gray-50 rounded-lg p-4">
            <h3 class="text-sm font-medium text-gray-500 mb-1">{{ metric.name }}</h3>
            <p class="text-2xl font-bold" :class="metric.color">{{ metric.value }}</p>
            <div class="flex items-center mt-2">
              <icon-trend-up v-if="metric.trend === 'up'" class="w-4 h-4 text-green-500 mr-1" />
              <icon-trend-down v-else class="w-4 h-4 text-red-500 mr-1" />
              <span :class="metric.trend === 'up' ? 'text-green-500' : 'text-red-500'">
                {{ metric.trendValue }}
              </span>
            </div>
          </div>
        </div>
        <div class="h-80">
          <canvas ref="financialChart"></canvas>
        </div>
      </div>
  
      <!-- Custom Reports -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4">Custom Reports</h2>
        <div class="flex flex-wrap gap-4 mb-6">
          <button v-for="report in reportTypes" :key="report"
                  @click="generateReport(report)"
                  class="px-4 py-2 bg-blue-100 text-blue-700 rounded-full hover:bg-blue-200 transition-colors">
            {{ report }}
          </button>
        </div>
        <div v-if="selectedReport" class="border rounded-lg p-4">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-lg font-medium">{{ selectedReport }} Report</h3>
            <div class="flex gap-2">
              <button @click="exportReport('csv')" class="px-3 py-1 bg-gray-100 rounded hover:bg-gray-200">
                Export CSV
              </button>
              <button @click="exportReport('pdf')" class="px-3 py-1 bg-gray-100 rounded hover:bg-gray-200">
                Export PDF
              </button>
              <button @click="exportReport('excel')" class="px-3 py-1 bg-gray-100 rounded hover:bg-gray-200">
                Export Excel
              </button>
            </div>
          </div>
          <!-- Placeholder for report data -->
          <table class="w-full">
            <thead>
              <tr class="bg-gray-50">
                <th class="p-2 text-left">Date</th>
                <th class="p-2 text-left">Category</th>
                <th class="p-2 text-left">Amount</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in reportData" :key="index" class="border-t">
                <td class="p-2">{{ item.date }}</td>
                <td class="p-2">{{ item.category }}</td>
                <td class="p-2">{{ formatCurrency(item.amount) }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <!-- AI-Powered Insights -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-xl font-semibold mb-4">AI-Powered Insights</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">Donation Trends</h3>
            <p class="text-gray-600 mb-4">
              Based on AI analysis, donations have increased by 15% compared to last quarter.
              The trend suggests a positive correlation with recent community outreach programs.
            </p>
            <div class="h-48">
              <canvas ref="donationTrendChart"></canvas>
            </div>
          </div>
          <div class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">Expenditure Optimization</h3>
            <p class="text-gray-600 mb-4">
              AI recommends reducing utility costs by 10% through energy-efficient upgrades,
              potentially saving $5,000 annually.
            </p>
            <ul class="list-disc list-inside text-sm text-gray-600">
              <li>Install LED lighting throughout the facility</li>
              <li>Upgrade to a smart thermostat system</li>
              <li>Conduct an energy audit for further savings opportunities</li>
            </ul>
          </div>
          <div class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">Attendance Patterns</h3>
            <p class="text-gray-600 mb-4">
              AI analysis shows a 20% increase in youth program attendance on Wednesdays.
              Consider expanding resources for this growing segment.
            </p>
            <div class="h-48">
              <canvas ref="attendanceChart"></canvas>
            </div>
          </div>
          <div class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">Fundraising Forecast</h3>
            <p class="text-gray-600 mb-4">
              Based on historical data and current trends, AI predicts the upcoming
              fundraising event could raise 25% more than last year's event.
            </p>
            <div class="flex justify-between items-center">
              <span class="text-sm text-gray-500">Predicted Amount:</span>
              <span class="text-lg font-bold text-green-600">$125,000</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-2.5 mt-2">
              <div class="bg-green-600 h-2.5 rounded-full" style="width: 75%"></div>
            </div>
            <div class="flex justify-between text-sm text-gray-500 mt-1">
              <span>$0</span>
              <span>$100,000</span>
              <span>$200,000</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import { Chart, registerables } from 'chart.js'
  // import { IconTrendUp, IconTrendDown } from '@iconify/vue'
  
  Chart.register(...registerables)
  
  const financialChart = ref<HTMLCanvasElement | null>(null)
  const donationTrendChart = ref<HTMLCanvasElement | null>(null)
  const attendanceChart = ref<HTMLCanvasElement | null>(null)
  
  const financialMetrics = ref([
    { name: 'Total Revenue', value: '$250,000', trend: 'up', trendValue: '8%', color: 'text-green-600' },
    { name: 'Total Expenses', value: '$180,000', trend: 'down', trendValue: '3%', color: 'text-red-600' },
    { name: 'Net Income', value: '$70,000', trend: 'up', trendValue: '12%', color: 'text-green-600' },
    { name: 'Cash Reserves', value: '$100,000', trend: 'up', trendValue: '5%', color: 'text-blue-600' },
  ])
  
  const reportTypes = ['Donations', 'Expenses', 'Payroll', 'Attendance']
  const selectedReport = ref('')
  const reportData = ref([])
  
  const generateReport = (type: string) => {
    selectedReport.value = type
    // Simulated report data
    reportData.value = [
      { date: '2025-03-01', category: 'Sunday Offering', amount: 5000 },
      { date: '2025-03-08', category: 'Sunday Offering', amount: 5500 },
      { date: '2025-03-15', category: 'Sunday Offering', amount: 4800 },
      { date: '2025-03-22', category: 'Sunday Offering', amount: 5200 },
      { date: '2025-03-29', category: 'Sunday Offering', amount: 5100 },
    ]
  }
  
  const exportReport = (format: string) => {
    console.log(`Exporting ${selectedReport.value} report in ${format} format`)
    // Implement export logic here
  }
  
  const formatCurrency = (amount: number) => {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(amount)
  }

  definePageMeta({
    layout: 'dashboard'
  })
  
  
  onMounted(() => {
    if (financialChart.value) {
      new Chart(financialChart.value, {
        type: 'line',
        data: {
          labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
          datasets: [
            {
              label: 'Revenue',
              data: [30000, 35000, 40000, 38000, 42000, 45000],
              borderColor: 'rgb(34, 197, 94)',
              tension: 0.1
            },
            {
              label: 'Expenses',
              data: [25000, 28000, 30000, 31000, 29000, 32000],
              borderColor: 'rgb(239, 68, 68)',
              tension: 0.1
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false
        }
      })
    }
  
    if (donationTrendChart.value) {
      new Chart(donationTrendChart.value, {
        type: 'bar',
        data: {
          labels: ['Q1', 'Q2', 'Q3', 'Q4'],
          datasets: [{
            label: 'Donations',
            data: [65000, 70000, 75000, 85000],
            backgroundColor: 'rgba(59, 130, 246, 0.5)'
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false
        }
      })
    }
  
    if (attendanceChart.value) {
      new Chart(attendanceChart.value, {
        type: 'line',
        data: {
          labels: ['Week 1', 'Week 2', 'Week 3', 'Week 4'],
          datasets: [{
            label: 'Youth Attendance',
            data: [50, 60, 75, 90],
            borderColor: 'rgb(249, 115, 22)',
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false
        }
      })
    }
  })
  </script>