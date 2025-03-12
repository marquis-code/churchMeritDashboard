<template>
    <div>
      <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
        <h1 class="text-2xl font-semibold text-gray-800 dark:text-white">
          Branch-Level Financial Reports
        </h1>
        <div class="mt-4 md:mt-0 flex flex-col sm:flex-row gap-3">
          <select 
            v-model="selectedBranch"
            class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            v-motion
            :initial="{ opacity: 0, y: 10 }"
            :enter="{ opacity: 1, y: 0, transition: { delay: 200 } }"
          >
            <option value="">All Branches</option>
            <option v-for="branch in branches" :key="branch.id" :value="branch.id">
              {{ branch.name }}
            </option>
          </select>
          <select 
            v-model="selectedPeriod"
            class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            v-motion
            :initial="{ opacity: 0, y: 10 }"
            :enter="{ opacity: 1, y: 0, transition: { delay: 300 } }"
          >
            <option value="month">Monthly</option>
            <option value="quarter">Quarterly</option>
            <option value="year">Yearly</option>
          </select>
          <button 
            @click="generateReport"
            class="px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors flex items-center justify-center"
            v-motion
            :initial="{ opacity: 0, y: 10 }"
            :enter="{ opacity: 1, y: 0, transition: { delay: 400 } }"
          >
            <DocumentChartBarIcon class="w-5 h-5 mr-2" />
            Generate Report
          </button>
        </div>
      </div>
  
      <!-- Financial Overview -->
      <div 
        class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 500 } }"
      >
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="flex items-center justify-between mb-4">
            <h3 class="text-lg font-semibold text-gray-800 dark:text-white">Total Income</h3>
            <div class="p-2 bg-green-100 rounded-full dark:bg-green-900">
              <BanknotesIcon class="w-6 h-6 text-green-600 dark:text-green-400" />
            </div>
          </div>
          <p class="text-3xl font-bold text-gray-800 dark:text-white">${{ formatNumber(totalIncome) }}</p>
          <div class="mt-2 flex items-center text-sm">
            <ArrowUpIcon class="w-4 h-4 text-green-500 mr-1" />
            <span class="text-green-500">{{ incomeGrowth }}% from previous period</span>
          </div>
        </div>
        
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="flex items-center justify-between mb-4">
            <h3 class="text-lg font-semibold text-gray-800 dark:text-white">Total Expenses</h3>
            <div class="p-2 bg-red-100 rounded-full dark:bg-red-900">
              <ReceiptPercentIcon class="w-6 h-6 text-red-600 dark:text-red-400" />
            </div>
          </div>
          <p class="text-3xl font-bold text-gray-800 dark:text-white">${{ formatNumber(totalExpenses) }}</p>
          <div class="mt-2 flex items-center text-sm">
            <ArrowUpIcon class="w-4 h-4 text-red-500 mr-1" />
            <span class="text-red-500">{{ expenseGrowth }}% from previous period</span>
          </div>
        </div>
        
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1">
          <div class="flex items-center justify-between mb-4">
            <h3 class="text-lg font-semibold text-gray-800 dark:text-white">Net Balance</h3>
            <div class="p-2 bg-blue-100 rounded-full dark:bg-blue-900">
              <ScaleIcon class="w-6 h-6 text-blue-600 dark:text-blue-400" />
            </div>
          </div>
          <p class="text-3xl font-bold text-gray-800 dark:text-white">${{ formatNumber(netBalance) }}</p>
          <div class="mt-2 flex items-center text-sm" :class="netBalance >= 0 ? 'text-green-500' : 'text-red-500'">
            <ArrowUpIcon v-if="netBalance >= 0" class="w-4 h-4 mr-1" />
            <ArrowDownIcon v-else class="w-4 h-4 mr-1" />
            <span>{{ Math.abs(balanceChange) }}% {{ netBalance >= 0 ? 'increase' : 'decrease' }}</span>
          </div>
        </div>
      </div>
  
      <!-- Income vs Expenses Chart -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-6"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 600 } }"
      >
        <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Income vs Expenses</h2>
        <div class="h-80">
          <FinancialChart :chartData="incomeVsExpensesData" />
        </div>
      </div>
  
      <!-- Branch Comparison -->
      <div 
        v-if="!selectedBranch"
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-6"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 700 } }"
      >
        <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Branch Comparison</h2>
        <div class="h-80">
          <BranchComparisonChart :chartData="branchComparisonData" />
        </div>
      </div>
  
      <!-- Financial Details Table -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 800 } }"
      >
        <div class="p-4 border-b dark:border-gray-700 flex flex-col md:flex-row md:items-center md:justify-between">
          <h2 class="text-lg font-semibold text-gray-800 dark:text-white">Financial Details</h2>
          <div class="mt-3 md:mt-0 flex items-center">
            <div class="relative">
              <input 
                type="text" 
                v-model="searchQuery"
                class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                placeholder="Search transactions..."
              />
              <MagnifyingGlassIcon class="absolute left-3 top-2.5 w-5 h-5 text-gray-400" />
            </div>
            <select 
              v-model="categoryFilter"
              class="ml-2 px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            >
              <option value="">All Categories</option>
              <option value="Donations">Donations</option>
              <option value="Tithes">Tithes</option>
              <option value="Events">Events</option>
              <option value="Salaries">Salaries</option>
              <option value="Utilities">Utilities</option>
              <option value="Maintenance">Maintenance</option>
              <option value="Programs">Programs</option>
            </select>
          </div>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="text-left text-gray-500 bg-gray-50 dark:bg-gray-700 dark:text-gray-300 border-b dark:border-gray-600">
                <th class="px-6 py-3 font-medium">Date</th>
                <th class="px-6 py-3 font-medium">Description</th>
                <th class="px-6 py-3 font-medium">Category</th>
                <th class="px-6 py-3 font-medium">Branch</th>
                <th class="px-6 py-3 font-medium">Amount</th>
                <th class="px-6 py-3 font-medium">Type</th>
              </tr>
            </thead>
            <tbody>
              <tr 
                v-for="(transaction, index) in filteredTransactions" 
                :key="index"
                class="border-b dark:border-gray-600 hover:bg-gray-50 dark:hover:bg-gray-700"
                v-motion
                :initial="{ opacity: 0, y: 10 }"
                :enter="{ opacity: 1, y: 0, transition: { delay: 900 + (index * 50) } }"
              >
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ transaction.date }}</td>
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ transaction.description }}</td>
                <td class="px-6 py-4">
                  <span 
                    class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium"
                    :class="{
                      'bg-purple-100 text-purple-800 dark:bg-purple-900 dark:text-purple-200': transaction.category === 'Donations',
                      'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-200': transaction.category === 'Tithes',
                      'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200': transaction.category === 'Events',
                      'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-200': transaction.category === 'Salaries',
                      'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-200': transaction.category === 'Utilities',
                      'bg-orange-100 text-orange-800 dark:bg-orange-900 dark:text-orange-200': transaction.category === 'Maintenance',
                      'bg-indigo-100 text-indigo-800 dark:bg-indigo-900 dark:text-indigo-200': transaction.category === 'Programs'
                    }"
                  >
                    {{ transaction.category }}
                  </span>
                </td>
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ transaction.branchName }}</td>
                <td class="px-6 py-4 font-medium" :class="transaction.type === 'Income' ? 'text-green-600 dark:text-green-400' : 'text-red-600 dark:text-red-400'">
                  {{ transaction.type === 'Income' ? '+' : '-' }}${{ formatNumber(transaction.amount) }}
                </td>
                <td class="px-6 py-4">
                  <span 
                    class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium"
                    :class="transaction.type === 'Income' ? 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200' : 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-200'"
                  >
                    {{ transaction.type }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        
        <!-- Empty State -->
        <div 
          v-if="filteredTransactions.length === 0"
          class="py-8 text-center"
        >
          <DocumentChartBarIcon class="w-12 h-12 mx-auto text-gray-400 dark:text-gray-500 mb-3" />
          <h3 class="text-lg font-medium text-gray-800 dark:text-white mb-2">No transactions found</h3>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            {{ searchQuery || categoryFilter ? 'No transactions match your search criteria.' : 'No financial data available for the selected period.' }}
          </p>
        </div>
      </div>
  
      <!-- Export Report Modal -->
      <Teleport to="body">
        <div v-if="showExportModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
          <div 
            class="w-full max-w-md p-6 mx-4 bg-white rounded-lg shadow-xl dark:bg-gray-800"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :enter="{ opacity: 1, scale: 1 }"
          >
            <div class="flex items-center justify-between mb-4">
              <h3 class="text-lg font-semibold text-gray-800 dark:text-white">
                Export Financial Report
              </h3>
              <button 
                @click="showExportModal = false"
                class="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
              >
                <XMarkIcon class="w-5 h-5" />
              </button>
            </div>
            <form @submit.prevent="exportReport">
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Report Format</label>
                <select 
                  v-model="exportForm.format"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="pdf">PDF Document</option>
                  <option value="excel">Excel Spreadsheet</option>
                  <option value="csv">CSV File</option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Include Charts</label>
                <div class="flex items-center">
                  <input 
                    type="checkbox" 
                    id="includeCharts"
                    v-model="exportForm.includeCharts"
                    class="w-4 h-4 text-purple-600 border-gray-300 rounded focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600"
                  />
                  <label for="includeCharts" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                    Include visual charts in the report
                  </label>
                </div>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Report Details</label>
                <div class="flex items-center mb-2">
                  <input 
                    type="checkbox" 
                    id="includeTransactions"
                    v-model="exportForm.includeTransactions"
                    class="w-4 h-4 text-purple-600 border-gray-300 rounded focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600"
                  />
                  <label for="includeTransactions" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                    Include detailed transactions
                  </label>
                </div>
                <div class="flex items-center">
                  <input 
                    type="checkbox" 
                    id="includeSummary"
                    v-model="exportForm.includeSummary"
                    class="w-4 h-4 text-purple-600 border-gray-300 rounded focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600"
                  />
                  <label for="includeSummary" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                    Include executive summary
                  </label>
                </div>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Email Report To</label>
                <input 
                  type="email" 
                  v-model="exportForm.email"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Enter email address (optional)"
                />
              </div>
              <div class="flex justify-end space-x-3">
                <button 
                  type="button"
                  @click="showExportModal = false"
                  class="px-4 py-2 text-sm font-medium text-gray-700 transition-colors bg-gray-200 rounded-md hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600"
                >
                  Cancel
                </button>
                <button 
                  type="submit"
                  class="px-4 py-2 text-sm font-medium text-white transition-colors bg-purple-600 rounded-md hover:bg-purple-700"
                >
                  Export Report
                </button>
              </div>
            </form>
          </div>
        </div>
      </Teleport>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, computed, onMounted } from 'vue';
  import { 
    DocumentChartBarIcon, 
    BanknotesIcon, 
    ReceiptPercentIcon, 
    ScaleIcon,
    ArrowUpIcon,
    ArrowDownIcon,
    MagnifyingGlassIcon,
    XMarkIcon
  } from '@heroicons/vue/24/outline';
  import { useToast } from '@/composables/useToast';
  
  // Types
  interface Branch {
    id: string;
    name: string;
    location: string;
  }
  
  interface Transaction {
    id: string;
    date: string;
    description: string;
    category: string;
    branchId: string;
    branchName: string;
    amount: number;
    type: 'Income' | 'Expense';
  }
  
  // State
  const branches = ref<Branch[]>([]);
  const transactions = ref<Transaction[]>([]);
  const selectedBranch = ref('');
  const selectedPeriod = ref('month');
  const searchQuery = ref('');
  const categoryFilter = ref('');
  const showExportModal = ref(false);
  const exportForm = ref({
    format: 'pdf',
    includeCharts: true,
    includeTransactions: true,
    includeSummary: true,
    email: ''
  });
  
  // Financial metrics
  const totalIncome = ref(0);
  const totalExpenses = ref(0);
  const netBalance = ref(0);
  const incomeGrowth = ref(0);
  const expenseGrowth = ref(0);
  const balanceChange = ref(0);
  
  // Chart data
  const incomeVsExpensesData = ref({
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    datasets: [
      {
        label: 'Income',
        data: [30000, 35000, 28000, 32000, 42000, 50000],
        backgroundColor: 'rgba(16, 185, 129, 0.2)',
        borderColor: 'rgb(16, 185, 129)',
        borderWidth: 2
      },
      {
        label: 'Expenses',
        data: [25000, 22000, 20000, 26000, 28000, 32000],
        backgroundColor: 'rgba(239, 68, 68, 0.2)',
        borderColor: 'rgb(239, 68, 68)',
        borderWidth: 2
      }
    ]
  });
  
  const branchComparisonData = ref({
    labels: ['Main Cathedral', 'Westside Chapel', 'Southside Community', 'Eastside Fellowship', 'Central City'],
    datasets: [
      {
        label: 'Income',
        data: [120000, 85000, 65000, 45000, 35000],
        backgroundColor: 'rgba(124, 58, 237, 0.7)'
      },
      {
        label: 'Expenses',
        data: [90000, 65000, 45000, 30000, 25000],
        backgroundColor: 'rgba(239, 68, 68, 0.7)'
      }
    ]
  });
  
  const { showToast } = useToast();
  
  // Computed
  const filteredTransactions = computed(() => {
    return transactions.value.filter(transaction => {
      const matchesSearch = searchQuery.value === '' || 
        transaction.description.toLowerCase().includes(searchQuery.value.toLowerCase());
      
      const matchesCategory = categoryFilter.value === '' || transaction.category === categoryFilter.value;
      
      const matchesBranch = selectedBranch.value === '' || transaction.branchId === selectedBranch.value;
      
      return matchesSearch && matchesCategory && matchesBranch;
    });
  });
  
  // Methods
  const fetchBranches = () => {
    // In a real app, this would be an API call
    branches.value = [
      { id: 'BR001', name: 'Main Cathedral', location: 'New York' },
      { id: 'BR002', name: 'Westside Chapel', location: 'Los Angeles' },
      { id: 'BR003', name: 'Southside Community Church', location: 'Miami' },
      { id: 'BR004', name: 'Eastside Fellowship', location: 'Boston' },
      { id: 'BR005', name: 'Central City Church', location: 'Chicago' }
    ];
  };
  
  const fetchTransactions = () => {
    // In a real app, this would be an API call
    transactions.value = [
      { id: 'TR001', date: '2025-03-01', description: 'Sunday Service Donations', category: 'Donations', branchId: 'BR001', branchName: 'Main Cathedral', amount: 5200, type: 'Income' },
      { id: 'TR002', date: '2025-03-03', description: 'Staff Salaries', category: 'Salaries', branchId: 'BR001', branchName: 'Main Cathedral', amount: 12500, type: 'Expense' },
      { id: 'TR003', date: '2025-03-05', description: 'Monthly Tithes', category: 'Tithes', branchId: 'BR002', branchName: 'Westside Chapel', amount: 8500, type: 'Income' },
      { id: 'TR004', date: '2025-03-07', description: 'Utility Bills', category: 'Utilities', branchId: 'BR002', branchName: 'Westside Chapel', amount: 1800, type: 'Expense' },
      { id: 'TR005', date: '2025-03-10', description: 'Youth Event Proceeds', category: 'Events', branchId: 'BR001', branchName: 'Main Cathedral', amount: 3200, type: 'Income' },
      { id: 'TR006', date: '2025-03-12', description: 'Building Maintenance', category: 'Maintenance', branchId: 'BR003', branchName: 'Southside Community Church', amount: 4500, type: 'Expense' },
      { id: 'TR007', date: '2025-03-15', description: 'Special Offering', category: 'Donations', branchId: 'BR003', branchName: 'Southside Community Church', amount: 6700, type: 'Income' },
      { id: 'TR008', date: '2025-03-18', description: 'Community Outreach Program', category: 'Programs', branchId: 'BR001', branchName: 'Main Cathedral', amount: 3500, type: 'Expense' },
      { id: 'TR009', date: '2025-03-20', description: 'Monthly Tithes', category: 'Tithes', branchId: 'BR001', branchName: 'Main Cathedral', amount: 12000, type: 'Income' },
      { id: 'TR010', date: '2025-03-22', description: 'Office Supplies', category: 'Maintenance', branchId: 'BR002', branchName: 'Westside Chapel', amount: 850, type: 'Expense' },
      { id: 'TR011', date: '2025-03-25', description: 'Fundraising Event', category: 'Events', branchId: 'BR003', branchName: 'Southside Community Church', amount: 9500, type: 'Income' },
      { id: 'TR012', date: '2025-03-28', description: 'Staff Training', category: 'Programs', branchId: 'BR001', branchName: 'Main Cathedral', amount: 2200, type: 'Expense' }
    ];
    
    // Calculate financial metrics
    calculateFinancialMetrics();
  };
  
  const calculateFinancialMetrics = () => {
    // Filter transactions by selected branch if needed
    const filteredTxns = selectedBranch.value ? 
      transactions.value.filter(t => t.branchId === selectedBranch.value) : 
      transactions.value;
    
    // Calculate totals
    const income = filteredTxns.filter(t => t.type === 'Income').reduce((sum, t) => sum + t.amount, 0);
    const expenses = filteredTxns.filter(t => t.type === 'Expense').reduce((sum, t) => sum + t.amount, 0);
    
    totalIncome.value = income;
    totalExpenses.value = expenses;
    netBalance.value = income - expenses;
    
    // Dummy growth percentages (in a real app, would compare to previous period)
    incomeGrowth.value = 8.5;
    expenseGrowth.value = 5.2;
    balanceChange.value = 12.3;
  };
  
  const generateReport = () => {
    // In a real app, this would fetch data based on the selected period and branch
    calculateFinancialMetrics();
    
    // Update chart data based on selection
    if (selectedBranch.value) {
      // Show data for just the selected branch
      const branchName = branches.value.find(b => b.id === selectedBranch.value)?.name || '';
      
      // Dummy data - in a real app, this would be dynamic
      if (selectedBranch.value === 'BR001') {
        incomeVsExpensesData.value.datasets[0].data = [35000, 40000, 32000, 38000, 45000, 55000];
        incomeVsExpensesData.value.datasets[1].data = [28000, 25000, 22000, 30000, 32000, 36000];
      } else if (selectedBranch.value === 'BR002') {
        incomeVsExpensesData.value.datasets[0].data = [25000, 28000, 22000, 26000, 30000, 35000];
        incomeVsExpensesData.value.datasets[1].data = [20000, 18000, 16000, 22000, 24000, 26000];
      } else {
        incomeVsExpensesData.value.datasets[0].data = [18000, 20000, 16000, 19000, 22000, 25000];
        incomeVsExpensesData.value.datasets[1].data = [15000, 14000, 12000, 16000, 18000, 20000];
      }
    } else {
      // Show aggregated data for all branches
      incomeVsExpensesData.value.datasets[0].data = [30000, 35000, 28000, 32000, 42000, 50000];
      incomeVsExpensesData.value.datasets[1].data = [25000, 22000, 20000, 26000, 28000, 32000];
    }
    
    showToast('Report generated successfully!', 'success');
  };
  
  const exportReport = () => {
    // In a real app, this would generate and export the report
    showToast(`Report exported as ${exportForm.format.toUpperCase()}!`, 'success');
    showExportModal.value = false;
  };
  
  const formatNumber = (num: number): string => {
    return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  };
  
  onMounted(() => {
    fetchBranches();
    fetchTransactions();
  });
  </script>