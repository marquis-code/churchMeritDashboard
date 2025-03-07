<!-- pages/facility-management/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold mb-6">Facility & Asset Management</h1>
  
      <!-- Facility Booking -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Facility Booking</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <!-- Booking Calendar -->
          <!-- <div>
            <VCalendar v-model:selected-date="selectedDate"
                       :attributes="bookingAttributes"
                       @dayclick="onDayClick">
              <template #day-content="{ day, attributes }">
                <div class="flex flex-col h-full">
                  <span class="text-sm">{{ day.day }}</span>
                  <div class="flex-grow">
                    <div v-for="attr in attributes" 
                         :key="attr.key" 
                         class="text-xs p-1 mb-1 rounded"
                         :class="attr.customData.class">
                      {{ attr.customData.title }}
                    </div>
                  </div>
                </div>
              </template>
            </VCalendar>
          </div> -->
  
          <!-- Booking Form -->
          <div>
            <form @submit.prevent="submitBooking" class="space-y-4">
              <div>
                <label class="block text-sm font-medium text-gray-700">Facility</label>
                <select v-model="booking.facility"
                        class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500">
                  <option value="hall">Main Hall</option>
                  <option value="classroom">Classroom</option>
                  <option value="vehicle">Church Vehicle</option>
                </select>
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700">Date</label>
                <input type="date" v-model="booking.date"
                       class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500">
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700">Time</label>
                <input type="time" v-model="booking.time"
                       class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500">
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700">Purpose</label>
                <textarea v-model="booking.purpose"
                          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500"
                          rows="3"></textarea>
              </div>
              <button type="submit"
                      class="w-full bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600">
                Submit Booking
              </button>
            </form>
          </div>
        </div>
      </div>
  
      <!-- Asset Tracking -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-2xl font-semibold mb-4">Asset Tracking</h2>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-gray-50">
                <th class="px-4 py-2 text-left">Asset</th>
                <th class="px-4 py-2 text-left">Category</th>
                <th class="px-4 py-2 text-left">Status</th>
                <th class="px-4 py-2 text-left">Last Maintenance</th>
                <th class="px-4 py-2 text-left">Next Maintenance</th>
                <th class="px-4 py-2 text-left">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="asset in assets" :key="asset.id" class="border-t">
                <td class="px-4 py-2">{{ asset.name }}</td>
                <td class="px-4 py-2">{{ asset.category }}</td>
                <td class="px-4 py-2">
                  <span :class="{
                    'bg-green-100 text-green-800': asset.status === 'Available',
                    'bg-red-100 text-red-800': asset.status === 'In Use',
                    'bg-yellow-100 text-yellow-800': asset.status === 'Maintenance'
                  }" class="px-2 py-1 rounded-full text-xs">
                    {{ asset.status }}
                  </span>
                </td>
                <td class="px-4 py-2">{{ asset.lastMaintenance }}</td>
                <td class="px-4 py-2">{{ asset.nextMaintenance }}</td>
                <td class="px-4 py-2">
                  <button @click="editAsset(asset)" class="text-blue-500 hover:text-blue-600 mr-2">
                    <icon-edit class="w-5 h-5" />
                  </button>
                  <button @click="scheduleMaintenanceModal(asset)" class="text-green-500 hover:text-green-600">
                    <icon-calendar class="w-5 h-5" />
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
  // import VCalendar from 'v-calendar'
  // import { IconEdit, IconCalendar } from '@iconify/vue'
  
  const selectedDate = ref(new Date())
  const booking = ref({
    facility: '',
    date: '',
    time: '',
    purpose: ''
  })

  definePageMeta({
    layout: 'dashboard'
  })
  
  const assets = ref([
    {
      id: 1,
      name: 'Projector',
      category: 'Electronics',
      status: 'Available',
      lastMaintenance: '2025-02-15',
      nextMaintenance: '2025-05-15'
    },
    {
      id: 2,
      name: 'Church Van',
      category: 'Vehicle',
      status: 'In Use',
      lastMaintenance: '2025-01-30',
      nextMaintenance: '2025-04-30'
    },
    // Add more assets...
  ])
  
  const bookingAttributes = ref([
    // Add booking attributes for the calendar
  ])
  
  const onDayClick = (day: any) => {
    console.log('Selected day:', day)
  }
  
  const submitBooking = () => {
    console.log('Submitting booking:', booking.value)
    // Implement booking submission logic
  }
  
  const editAsset = (asset: any) => {
    console.log('Editing asset:', asset)
    // Implement asset editing logic
  }
  
  const scheduleMaintenanceModal = (asset: any) => {
    console.log('Scheduling maintenance for:', asset)
    // Implement maintenance scheduling logic
  }
  </script>