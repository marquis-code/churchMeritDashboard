<template>
    <div>
      <div class="md:flex md:items-center md:justify-between">
        <div class="flex-1 min-w-0">
          <h2 class="text-2xl font-bold leading-7 text-gray-900 sm:text-3xl sm:truncate">Church Profile Setup</h2>
        </div>
        <div class="mt-4 flex md:mt-0 md:ml-4">
          <button type="button" class="ml-3 inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Save Changes
          </button>
        </div>
      </div>
  
      <div class="mt-8 bg-white shadow overflow-hidden sm:rounded-lg">
        <div class="px-4 py-5 sm:px-6">
          <h3 class="text-lg leading-6 font-medium text-gray-900">Church Information</h3>
          <p class="mt-1 max-w-2xl text-sm text-gray-500">Update your church details and configuration.</p>
        </div>
        <div class="border-t border-gray-200 px-4 py-5 sm:p-0">
          <dl class="sm:divide-y sm:divide-gray-200">
            <!-- Church Name and Logo -->
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
              <dt class="text-sm font-medium text-gray-500">Church Name & Logo</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <div class="flex items-center">
                  <div class="flex-shrink-0 h-20 w-20">
                    <!-- <img class="h-20 w-20 rounded-full" src="/placeholder.svg?height=80&width=80" alt="Church Logo" /> -->
                  </div>
                  <div class="ml-4 flex-1">
                    <div class="relative rounded-md shadow-sm">
                      <input type="text" v-model="churchProfile.name" class="focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" placeholder="Church Name" />
                    </div>
                    <div class="mt-2">
                      <label class="inline-flex items-center px-3 py-2 border border-gray-300 shadow-sm text-sm leading-4 font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        <svg class="mr-2 -ml-1 h-4 w-4 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                        </svg>
                        Upload Logo
                        <input type="file" class="sr-only" />
                      </label>
                    </div>
                  </div>
                </div>
              </dd>
            </div>
  
            <!-- Church Address -->
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
              <dt class="text-sm font-medium text-gray-500">Address</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <div class="grid grid-cols-6 gap-6">
                  <div class="col-span-6">
                    <label for="street-address" class="block text-sm font-medium text-gray-700">Street address</label>
                    <input type="text" v-model="churchProfile.address.street" id="street-address" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
  
                  <div class="col-span-6 sm:col-span-6 lg:col-span-2">
                    <label for="city" class="block text-sm font-medium text-gray-700">City</label>
                    <input type="text" v-model="churchProfile.address.city" id="city" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
  
                  <div class="col-span-6 sm:col-span-3 lg:col-span-2">
                    <label for="state" class="block text-sm font-medium text-gray-700">State / Province</label>
                    <input type="text" v-model="churchProfile.address.state" id="state" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
  
                  <div class="col-span-6 sm:col-span-3 lg:col-span-2">
                    <label for="postal-code" class="block text-sm font-medium text-gray-700">ZIP / Postal code</label>
                    <input type="text" v-model="churchProfile.address.postalCode" id="postal-code" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
                </div>
              </dd>
            </div>
  
            <!-- Multi-branch Setup -->
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
              <dt class="text-sm font-medium text-gray-500">Branches</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <div class="space-y-4">
                  <div v-for="(branch, index) in churchProfile.branches" :key="index" class="flex items-center space-x-4">
                    <div class="flex-1">
                      <input type="text" v-model="branch.name" class="focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" placeholder="Branch Name" />
                    </div>
                    <div class="flex-1">
                      <input type="text" v-model="branch.location" class="focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" placeholder="Location" />
                    </div>
                    <button type="button" @click="removeBranch(index)" class="inline-flex items-center p-1.5 border border-transparent rounded-full shadow-sm text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500">
                      <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                        <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
                      </svg>
                    </button>
                  </div>
                  <button type="button" @click="addBranch" class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                    <svg class="mr-2 -ml-1 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                      <path fill-rule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clip-rule="evenodd" />
                    </svg>
                    Add Branch
                  </button>
                </div>
              </dd>
            </div>
  
            <!-- Financial Configuration -->
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
              <dt class="text-sm font-medium text-gray-500">Financial Configuration</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <div class="space-y-4">
                  <div>
                    <label for="bank-name" class="block text-sm font-medium text-gray-700">Bank Name</label>
                    <input type="text" v-model="churchProfile.financials.bankName" id="bank-name" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
                  <div>
                    <label for="account-number" class="block text-sm font-medium text-gray-700">Account Number</label>
                    <input type="text" v-model="churchProfile.financials.accountNumber" id="account-number" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                  </div>
                  <div>
                    <label for="payment-methods" class="block text-sm font-medium text-gray-700">Payment Methods</label>
                    <div class="mt-2 space-y-2">
                      <div class="flex items-center">
                        <input id="payment-bank" v-model="churchProfile.financials.paymentMethods" value="bank" type="checkbox" class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded" />
                        <label for="payment-bank" class="ml-2 block text-sm text-gray-900">Bank Transfer</label>
                      </div>
                      <div class="flex items-center">
                        <input id="payment-card" v-model="churchProfile.financials.paymentMethods" value="card" type="checkbox" class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded" />
                        <label for="payment-card" class="ml-2 block text-sm text-gray-900">Credit/Debit Card</label>
                      </div>
                      <div class="flex items-center">
                        <input id="payment-ussd" v-model="churchProfile.financials.paymentMethods" value="ussd" type="checkbox" class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded" />
                        <label for="payment-ussd" class="ml-2 block text-sm text-gray-900">USSD</label>
                      </div>
                      <div class="flex items-center">
                        <input id="payment-mobile" v-model="churchProfile.financials.paymentMethods" value="mobile" type="checkbox" class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded" />
                        <label for="payment-mobile" class="ml-2 block text-sm text-gray-900">Mobile Wallets</label>
                      </div>
                    </div>
                  </div>
                </div>
              </dd>
            </div>
          </dl>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const churchProfile = ref({
    name: 'Grace Community Church',
    address: {
      street: '123 Main Street',
      city: 'Anytown',
      state: 'State',
      postalCode: '12345'
    },
    branches: [
      { name: 'Main Campus', location: 'Downtown' },
      { name: 'North Campus', location: 'North Hills' }
    ],
    financials: {
      bankName: 'First National Bank',
      accountNumber: '1234567890',
      paymentMethods: ['bank', 'card']
    }
  })

  definePageMeta({
    layout: 'dashboard'
  })
  
  
  const addBranch = () => {
    churchProfile.value.branches.push({ name: '', location: '' })
  }
  
  const removeBranch = (index: number) => {
    churchProfile.value.branches.splice(index, 1)
  }

  definePageMeta({
      layout: 'dashboard'
  })
  </script>