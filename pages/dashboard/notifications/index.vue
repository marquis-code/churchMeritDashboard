<!-- pages/notifications/index.vue -->
<template>
    <div class="container mx-auto px-4">
      <h1 class="text-2xl font-bold mb-6">Notifications & Alerts</h1>
  
      <!-- Notification Center -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4">Notification Center</h2>
        <div class="space-y-4">
          <div v-for="notification in notifications" :key="notification.id" 
               class="flex items-start p-4 border rounded-lg hover:bg-gray-50 transition-colors">
            <div class="flex-shrink-0 mr-4">
              <icon-bell v-if="notification.type === 'alert'" class="w-6 h-6 text-yellow-500" />
              <icon-mail v-else-if="notification.type === 'message'" class="w-6 h-6 text-blue-500" />
              <icon-info v-else class="w-6 h-6 text-gray-500" />
            </div>
            <div class="flex-grow">
              <h3 class="font-medium">{{ notification.title }}</h3>
              <p class="text-gray-600">{{ notification.message }}</p>
              <p class="text-sm text-gray-500 mt-1">{{ formatDate(notification.timestamp) }}</p>
            </div>
            <button @click="markAsRead(notification.id)" class="text-blue-500 hover:text-blue-600">
              Mark as read
            </button>
          </div>
        </div>
      </div>
  
      <!-- Notification Preferences -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-xl font-semibold mb-4">Notification Preferences</h2>
        <div class="space-y-6">
          <div v-for="category in notificationCategories" :key="category.id" class="border-b pb-4">
            <h3 class="font-medium mb-2">{{ category.name }}</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div v-for="setting in category.settings" :key="setting.id" class="flex items-center justify-between">
                <span>{{ setting.name }}</span>
                <div class="flex items-center space-x-4">
                  <label class="flex items-center cursor-pointer">
                    <input type="checkbox" v-model="setting.email" class="sr-only peer">
                    <div class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
                    <span class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Email</span>
                  </label>
                  <label class="flex items-center cursor-pointer">
                    <input type="checkbox" v-model="setting.sms" class="sr-only peer">
                    <div class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
                    <span class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">SMS</span>
                  </label>
                </div>
              </div>
            </div>
          </div>
        </div>
        <button @click="savePreferences" class="mt-6 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors">
          Save Preferences
        </button>
      </div>
  
      <!-- In-app Notification Settings -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-xl font-semibold mb-4">In-app Notification Settings</h2>
        <div class="space-y-4">
          <div v-for="setting in inAppSettings" :key="setting.id" class="flex items-center justify-between">
            <span>{{ setting.name }}</span>
            <label class="flex items-center cursor-pointer">
              <input type="checkbox" v-model="setting.enabled" class="sr-only peer">
              <div class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
              <span class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Enabled</span>
            </label>
          </div>
        </div>
        <button @click="saveInAppSettings" class="mt-6 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors">
          Save In-app Settings
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  // import { IconBell, IconMail, IconInfo } from '@iconify/vue'
  
  const notifications = ref([
    {
      id: 1,
      type: 'alert',
      title: 'New Donation Received',
      message: 'A donation of $500 has been received from John Doe.',
      timestamp: new Date('2025-03-06T10:30:00')
    },
    {
      id: 2,
      type: 'message',
      title: 'Upcoming Event Reminder',
      message: 'Don\'t forget about the charity fundraiser this Saturday at 2 PM.',
      timestamp: new Date('2025-03-05T15:45:00')
    },
    {
      id: 3,
      type: 'info',
      title: 'System Maintenance',
      message: 'The system will be undergoing maintenance on Sunday from 2 AM to 4 AM.',
      timestamp: new Date('2025-03-04T09:00:00')
    }
  ])
  
  const notificationCategories = ref([
    {
      id: 1,
      name: 'Donations',
      settings: [
        { id: 1, name: 'New Donations', email: true, sms: false },
        { id: 2, name: 'Monthly Summary', email: true, sms: false }
      ]
    },
    {
      id: 2,
      name: 'Events',
      settings: [
        { id: 3, name: 'Event Reminders', email: true, sms: true },
        { id: 4, name: 'Event Changes', email: true, sms: false }
      ]
    },
    {
      id: 3,
      name: 'Financial',
      settings: [
        { id: 5, name: 'Bill Due Reminders', email: true, sms: true },
        { id: 6, name: 'Financial Reports', email: true, sms: false }
      ]
    }
  ])
  
  const inAppSettings = ref([
    { id: 1, name: 'Pending Approvals', enabled: true },
    { id: 2, name: 'Remittance Notifications', enabled: true },
    { id: 3, name: 'Fund Transfer Alerts', enabled: true },
    { id: 4, name: 'System Updates', enabled: false }
  ])
  
  const formatDate = (date: Date) => {
    return date.toLocaleString('en-US', {
      year: 'numeric',
      month: 'short',
      day: 'numeric',
      hour: '2-digit',
      minute: '2-digit'
    })
  }
  
  const markAsRead = (id: number) => {
    const index = notifications.value.findIndex(n => n.id === id)
    if (index !== -1) {
      notifications.value.splice(index, 1)
    }
  }

  definePageMeta({
    layout: 'dashboard'
  })
  
  const savePreferences = () => {
    console.log('Saving notification preferences:', notificationCategories.value)
    // Implement the logic to save preferences to the backend
  }
  
  const saveInAppSettings = () => {
    console.log('Saving in-app notification settings:', inAppSettings.value)
    // Implement the logic to save in-app settings to the backend
  }
  </script>