<!-- pages/payments-donations/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold">Payments & Donations</h1>
  
      <!-- Payment Collection Page -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Make a Donation</h2>
        <form @submit.prevent="submitDonation" class="space-y-6">
          <div>
            <label class="block text-sm font-medium text-gray-700">Donation Amount</label>
            <div class="mt-1 relative rounded-md shadow-sm">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <span class="text-gray-500 sm:text-sm">$</span>
              </div>
              <input type="number" v-model="donation.amount" min="0" step="0.01"
                     class="focus:ring-blue-500 focus:border-blue-500 block w-full pl-7 pr-12 sm:text-sm border-gray-300 rounded-md"
                     placeholder="0.00">
            </div>
          </div>
  
          <div>
            <label class="block text-sm font-medium text-gray-700">Donation Category</label>
            <select v-model="donation.category"
                    class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm">
              <option value="tithe">Tithe</option>
              <option value="offering">Offering</option>
              <option value="missions">Missions</option>
              <option value="building">Building Fund</option>
            </select>
          </div>
  
          <div>
            <label class="block text-sm font-medium text-gray-700">Payment Method</label>
            <div class="mt-2 space-y-2">
              <label class="inline-flex items-center">
                <input type="radio" v-model="donation.paymentMethod" value="card" class="form-radio">
                <span class="ml-2">Credit/Debit Card</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" v-model="donation.paymentMethod" value="bank" class="form-radio">
                <span class="ml-2">Bank Transfer</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" v-model="donation.paymentMethod" value="mobile" class="form-radio">
                <span class="ml-2">Mobile Wallet</span>
              </label>
            </div>
          </div>
  
          <div v-if="donation.paymentMethod === 'card'">
            <!-- Credit Card Input Fields -->
            <div class="space-y-2">
              <input type="text" placeholder="Card Number" class="w-full px-3 py-2 border rounded-md">
              <div class="flex space-x-2">
                <input type="text" placeholder="MM/YY" class="w-1/2 px-3 py-2 border rounded-md">
                <input type="text" placeholder="CVV" class="w-1/2 px-3 py-2 border rounded-md">
              </div>
            </div>
          </div>
  
          <div>
            <label class="inline-flex items-center">
              <input type="checkbox" v-model="donation.recurring" class="form-checkbox">
              <span class="ml-2">Make this a recurring donation</span>
            </label>
          </div>
  
          <div v-if="donation.recurring">
            <label class="block text-sm font-medium text-gray-700">Recurrence</label>
            <select v-model="donation.recurrenceInterval"
                    class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm">
              <option value="weekly">Weekly</option>
              <option value="monthly">Monthly</option>
              <option value="quarterly">Quarterly</option>
            </select>
          </div>
  
          <button type="submit" class="w-full bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600">
            Submit Donation
          </button>
        </form>
      </div>
  
      <!-- Payment History & Receipts -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-2xl font-semibold mb-4">Payment History & Receipts</h2>
        <div class="mb-4 flex space-x-2">
          <input type="text" v-model="searchQuery" placeholder="Search by donor or amount" 
                 class="px-3 py-2 border rounded-md flex-grow">
          <button @click="searchTransactions" class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600">
            Search
          </button>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-gray-50">
                <th class="px-4 py-2 text-left">Date</th>
                <th class="px-4 py-2 text-left">Donor</th>
                <th class="px-4 py-2 text-left">Amount</th>
                <th class="px-4 py-2 text-left">Category</th>
                <th class="px-4 py-2 text-left">Status</th>
                <th class="px-4 py-2 text-left">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="transaction in transactions" :key="transaction.id" class="border-t">
                <td class="px-4 py-2">{{ transaction.date }}</td>
                <td class="px-4 py-2">{{ transaction.donor }}</td>
                <td class="px-4 py-2">${{ transaction.amount.toFixed(2) }}</td>
                <td class="px-4 py-2">{{ transaction.category }}</td>
                <td class="px-4 py-2">
                  <span :class="{
                    'bg-green-100 text-green-800': transaction.status === 'Completed',
                    'bg-yellow-100 text-yellow-800': transaction.status === 'Pending',
                    'bg-red-100 text-red-800': transaction.status === 'Failed'
                  }" class="px-2 py-1 rounded-full text-xs">
                    {{ transaction.status }}
                  </span>
                </td>
                <td class="px-4 py-2">
                  <button @click="viewReceipt(transaction)" class="text-blue-500 hover:text-blue-600 mr-2">
                    <icon-receipt class="w-5 h-5" />
                  </button>
                  <button v-if="transaction.status === 'Completed'" @click="emailReceipt(transaction)" class="text-green-500 hover:text-green-600">
                    <icon-mail class="w-5 h-5" />
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  // import { IconReceipt, IconMail } from '@iconify/vue'
  
  const donation = ref({
    amount: 0,
    category: 'tithe',
    paymentMethod: 'card',
    recurring: false,
    recurrenceInterval: 'monthly'
  })
  
  const searchQuery = ref('')

  definePageMeta({
    layout: 'dashboard'
  })
  
  const transactions = ref([
    {
      id: 1,
      date: '2025-03-01',
      donor: 'John Doe',
      amount: 100.00,
      category: 'Tithe',
      status: 'Completed'
    },
    {
      id: 2,
      date: '2025-03-05',
      donor: 'Jane Smith',
      amount: 50.00,
      category: 'Offering',
      status: 'Pending'
    },
    // Add more transactions...
  ])
  
  const submitDonation = () => {
    console.log('Submitting donation:', donation.value)
    // Implement donation submission logic
  }
  
  const searchTransactions = () => {
    console.log('Searching transactions:', searchQuery.value)
    // Implement transaction search logic
  }
  
  const viewReceipt = (transaction: any) => {
    console.log('Viewing receipt for transaction:', transaction)
    // Implement receipt viewing logic
  }
  
  const emailReceipt = (transaction: any) => {
    console.log('Emailing receipt for transaction:', transaction)
    // Implement email receipt logic
  }
  </script>