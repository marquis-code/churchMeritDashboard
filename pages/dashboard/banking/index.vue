<!-- pages/banking/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold">Banking & Financial Management</h1>
      
      <!-- Bank Account Management -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
        <!-- Account Summary Cards -->
        <div v-for="account in bankAccounts" :key="account.id" 
             class="bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow">
          <div class="flex justify-between items-start mb-4">
            <div>
              <h3 class="text-lg font-semibold">{{ account.name }}</h3>
              <p class="text-gray-600 text-sm">{{ account.accountNumber }}</p>
            </div>
            <span :class="[
              'px-2 py-1 rounded-full text-sm',
              account.type === 'Main' ? 'bg-blue-100 text-blue-800' : 'bg-gray-100 text-gray-800'
            ]">
              {{ account.type }}
            </span>
          </div>
          <div class="mb-4">
            <p class="text-2xl font-bold">{{ formatCurrency(account.balance) }}</p>
            <p class="text-gray-600 text-sm">Available Balance</p>
          </div>
          <div class="flex gap-2">
            <button @click="initiateTransfer(account)" 
                    class="flex-1 bg-blue-500 text-white px-3 py-2 rounded hover:bg-blue-600">
              Transfer
            </button>
            <button @click="viewTransactions(account)"
                    class="flex-1 border border-gray-300 px-3 py-2 rounded hover:bg-gray-50">
              History
            </button>
          </div>
        </div>
  
        <!-- Add New Account Card -->
        <div @click="showAddAccountModal = true"
             class="bg-gray-50 rounded-lg border-2 border-dashed border-gray-300 p-6 flex flex-col items-center justify-center cursor-pointer hover:bg-gray-100 transition-colors">
          <icon-plus-circle class="w-12 h-12 text-gray-400 mb-2" />
          <p class="text-gray-600 font-medium">Add New Account</p>
        </div>
      </div>
  
      <!-- Recent Transactions -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-xl font-semibold">Recent Transactions</h2>
          <button class="text-blue-500 hover:text-blue-600">View All</button>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="border-b">
                <th class="text-left p-3">Date</th>
                <th class="text-left p-3">Description</th>
                <th class="text-left p-3">Account</th>
                <th class="text-right p-3">Amount</th>
                <th class="text-left p-3">Status</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="transaction in recentTransactions" 
                  :key="transaction.id"
                  class="border-b hover:bg-gray-50">
                <td class="p-3">{{ formatDate(transaction.date) }}</td>
                <td class="p-3">{{ transaction.description }}</td>
                <td class="p-3">{{ transaction.account }}</td>
                <td class="p-3 text-right" :class="transaction.amount > 0 ? 'text-green-600' : 'text-red-600'">
                  {{ formatCurrency(transaction.amount) }}
                </td>
                <td class="p-3">
                  <span :class="[
                    'px-2 py-1 rounded-full text-sm',
                    getStatusClass(transaction.status)
                  ]">
                    {{ transaction.status }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <!-- Bill Payments Section -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-xl font-semibold mb-6">Bill Payments</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
          <div v-for="bill in upcomingBills" 
               :key="bill.id"
               class="border rounded-lg p-4 hover:shadow-md transition-shadow">
            <div class="flex items-center gap-3 mb-3">
              <icon-lightning class="w-6 h-6 text-yellow-500" v-if="bill.type === 'Electricity'" />
              <icon-wifi class="w-6 h-6 text-blue-500" v-if="bill.type === 'Internet'" />
              <icon-droplet class="w-6 h-6 text-blue-400" v-if="bill.type === 'Water'" />
              <div>
                <h3 class="font-medium">{{ bill.provider }}</h3>
                <p class="text-sm text-gray-600">Due: {{ formatDate(bill.dueDate) }}</p>
              </div>
            </div>
            <div class="flex justify-between items-center">
              <span class="font-bold">{{ formatCurrency(bill.amount) }}</span>
              <button class="text-blue-500 hover:text-blue-600">Pay Now</button>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Add Account Modal -->
      <TransitionRoot appear :show="showAddAccountModal" as="template">
        <Dialog as="div" @close="showAddAccountModal = false" class="relative z-50">
          <TransitionChild
            enter="duration-300 ease-out"
            enter-from="opacity-0"
            enter-to="opacity-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100"
            leave-to="opacity-0"
          >
            <div class="fixed inset-0 bg-black/25" />
          </TransitionChild>
  
          <div class="fixed inset-0 overflow-y-auto">
            <div class="flex min-h-full items-center justify-center p-4">
              <TransitionChild
                enter="duration-300 ease-out"
                enter-from="opacity-0 scale-95"
                enter-to="opacity-100 scale-100"
                leave="duration-200 ease-in"
                leave-from="opacity-100 scale-100"
                leave-to="opacity-0 scale-95"
              >
                <DialogPanel class="w-full max-w-md bg-white rounded-lg p-6">
                  <DialogTitle class="text-lg font-medium mb-4">Add New Bank Account</DialogTitle>
                  <form @submit.prevent="addNewAccount" class="space-y-4">
                    <div>
                      <label class="block text-sm font-medium text-gray-700">Account Name</label>
                      <input type="text" v-model="newAccount.name" 
                             class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500" />
                    </div>
                    <div>
                      <label class="block text-sm font-medium text-gray-700">Account Number</label>
                      <input type="text" v-model="newAccount.accountNumber"
                             class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500" />
                    </div>
                    <div>
                      <label class="block text-sm font-medium text-gray-700">Account Type</label>
                      <select v-model="newAccount.type"
                              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500">
                        <option value="Main">Main</option>
                        <option value="Savings">Savings</option>
                        <option value="Project">Project</option>
                      </select>
                    </div>
                    <div class="flex justify-end gap-3 mt-6">
                      <button type="button" 
                              @click="showAddAccountModal = false"
                              class="px-4 py-2 border rounded-md hover:bg-gray-50">
                        Cancel
                      </button>
                      <button type="submit"
                              class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600">
                        Add Account
                      </button>
                    </div>
                  </form>
                </DialogPanel>
              </TransitionChild>
            </div>
          </div>
        </Dialog>
      </TransitionRoot>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  import { Dialog, DialogPanel, DialogTitle, TransitionRoot, TransitionChild } from '@headlessui/vue'
  // import { 
  //   IconPlusCircle, 
  //   IconLightning, 
  //   IconWifi, 
  //   IconDroplet 
  // } from '@iconify/vue'

  definePageMeta({
      layout: 'dashboard'
  })
  
  const showAddAccountModal = ref(false)
  
  const bankAccounts = ref([
    {
      id: 1,
      name: 'Main Church Account',
      accountNumber: '**** 1234',
      type: 'Main',
      balance: 50000.00
    },
    {
      id: 2,
      name: 'Building Fund',
      accountNumber: '**** 5678',
      type: 'Project',
      balance: 125000.00
    },
    {
      id: 3,
      name: 'Mission Fund',
      accountNumber: '**** 9012',
      type: 'Savings',
      balance: 75000.00
    }
  ])
  
  const recentTransactions = ref([
    {
      id: 1,
      date: '2025-03-06',
      description: 'Utility Payment',
      account: 'Main Church Account',
      amount: -1500.00,
      status: 'Completed'
    },
    {
      id: 2,
      date: '2025-03-05',
      description: 'Sunday Offering',
      account: 'Main Church Account',
      amount: 8500.00,
      status: 'Completed'
    },
    {
      id: 3,
      date: '2025-03-05',
      description: 'Building Fund Transfer',
      account: 'Building Fund',
      amount: 25000.00,
      status: 'Pending'
    }
  ])
  
  const upcomingBills = ref([
    {
      id: 1,
      type: 'Electricity',
      provider: 'Power Corp',
      amount: 2500.00,
      dueDate: '2025-03-15'
    },
    {
      id: 2,
      type: 'Water',
      provider: 'Water Services',
      amount: 1200.00,
      dueDate: '2025-03-18'
    },
    {
      id: 3,
      type: 'Internet',
      provider: 'FastNet ISP',
      amount: 800.00,
      dueDate: '2025-03-20'
    }
  ])
  
  const newAccount = ref({
    name: '',
    accountNumber: '',
    type: 'Main'
  })
  
  const formatCurrency = (amount: number) => {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(amount)
  }
  
  const formatDate = (date: string) => {
    return new Date(date).toLocaleDateString('en-US', {
      year: 'numeric',
      month: 'short',
      day: 'numeric'
    })
  }
  
  const getStatusClass = (status: string) => {
    switch (status) {
      case 'Completed':
        return 'bg-green-100 text-green-800'
      case 'Pending':
        return 'bg-yellow-100 text-yellow-800'
      default:
        return 'bg-gray-100 text-gray-800'
    }
  }
  
  const initiateTransfer = (account: any) => {
    // Implement transfer logic
    console.log('Initiating transfer from account:', account)
  }
  
  const viewTransactions = (account: any) => {
    // Implement view transactions logic
    console.log('Viewing transactions for account:', account)
  }
  
  const addNewAccount = () => {
    // Implement add account logic
    console.log('Adding new account:', newAccount.value)
    showAddAccountModal.value = false
  }
  </script>