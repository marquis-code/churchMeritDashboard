<!-- pages/legal-compliance/index.vue -->
<template>
    <div class="container mx-auto">
      <h1 class="text-2xl font-bold mb-6">Legal & Compliance Management</h1>
  
      <!-- Document Storage -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h2 class="text-2xl font-semibold mb-4">Document Storage</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div v-for="category in documentCategories" :key="category.id" class="border rounded-lg p-4">
            <h3 class="text-lg font-medium mb-2">{{ category.name }}</h3>
            <ul class="space-y-2">
              <li v-for="document in category.documents" :key="document.id" class="flex justify-between items-center">
                <span class="text-sm">{{ document.name }}</span>
                <div class="flex space-x-2">
                  <button @click="viewDocument(document)" class="text-blue-500 hover:text-blue-600">
                    <icon-eye class="w-5 h-5" />
                  </button>
                  <button @click="editDocument(document)" class="text-green-500 hover:text-green-600">
                    <icon-edit class="w-5 h-5" />
                  </button>
                </div>
              </li>
            </ul>
            <button @click="addDocument(category)" class="mt-4 text-blue-500 hover:text-blue-600 flex items-center">
              <icon-plus class="w-5 h-5 mr-1" />
              Add Document
            </button>
          </div>
        </div>
      </div>
  
      <!-- Regulatory Compliance Tracking -->
      <div class="bg-white rounded-lg shadow-md p-6">
        <h2 class="text-2xl font-semibold mb-4">Regulatory Compliance Tracking</h2>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-gray-50">
                <th class="px-4 py-2 text-left">Compliance Item</th>
                <th class="px-4 py-2 text-left">Type</th>
                <th class="px-4 py-2 text-left">Due Date</th>
                <th class="px-4 py-2 text-left">Status</th>
                <th class="px-4 py-2 text-left">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in complianceItems" :key="item.id" class="border-t">
                <td class="px-4 py-2">{{ item.name }}</td>
                <td class="px-4 py-2">{{ item.type }}</td>
                <td class="px-4 py-2">{{ item.dueDate }}</td>
                <td class="px-4 py-2">
                  <span :class="{
                    'bg-green-100 text-green-800': item.status === 'Completed',
                    'bg-yellow-100 text-yellow-800': item.status === 'Pending',
                    'bg-red-100 text-red-800': item.status === 'Overdue'
                  }" class="px-2 py-1 rounded-full text-xs">
                    {{ item.status }}
                  </span>
                </td>
                <td class="px-4 py-2">
                  <button @click="updateComplianceStatus(item)" class="text-blue-500 hover:text-blue-600 mr-2">
                    <icon-check-circle class="w-5 h-5" />
                  </button>
                  <button @click="viewComplianceDetails(item)" class="text-green-500 hover:text-green-600">
                    <icon-info class="w-5 h-5" />
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
  // import { IconEye, IconEdit, IconPlus, IconCheckCircle, IconInfo } from '@iconify/vue'
  
  definePageMeta({
    layout: 'dashboard'
  })
  

  const documentCategories = ref([
    {
      id: 1,
      name: 'Church Policies',
      documents: [
        { id: 1, name: 'Child Protection Policy' },
        { id: 2, name: 'Financial Management Policy' },
      ]
    },
    {
      id: 2,
      name: 'Legal Agreements',
      documents: [
        { id: 3, name: 'Lease Agreement' },
        { id: 4, name: 'Employment Contracts' },
      ]
    },
    {
      id: 3,
      name: 'Compliance Documents',
      documents: [
        { id: 5, name: 'Annual Financial Report' },
        { id: 6, name: 'Tax Exemption Certificate' },
      ]
    }
  ])
  
  const complianceItems = ref([
    {
      id: 1,
      name: 'Annual Tax Filing',
      type: 'Financial',
      dueDate: '2025-04-15',
      status: 'Pending'
    },
    {
      id: 2,
      name: 'Board Meeting Minutes',
      type: 'Legal',
      dueDate: '2025-03-31',
      status: 'Completed'
    },
    {
      id: 3,
      name: 'Fire Safety Inspection',
      type: 'Safety',
      dueDate: '2025-05-30',
      status: 'Overdue'
    }
  ])
  
  const viewDocument = (document: any) => {
    console.log('Viewing document:', document)
    // Implement document viewing logic
  }
  
  const editDocument = (document: any) => {
    console.log('Editing document:', document)
    // Implement document editing logic
  }
  
  const addDocument = (category: any) => {
    console.log('Adding document to category:', category)
    // Implement document addition logic
  }
  
  const updateComplianceStatus = (item: any) => {
    console.log('Updating compliance status for:', item)
    // Implement compliance status update logic
  }
  
  const viewComplianceDetails = (item: any) => {
    console.log('Viewing compliance details for:', item)
    // Implement compliance details viewing logic
  }
  </script>