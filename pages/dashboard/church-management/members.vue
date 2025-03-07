<template>
    <div>
      <div class="md:flex md:items-center md:justify-between">
        <div class="flex-1 min-w-0">
          <h2 class="text-2xl font-bold leading-7 text-gray-900 sm:text-3xl sm:truncate">Member & Staff Management</h2>
        </div>
        <div class="mt-4 flex md:mt-0 md:ml-4">
          <button @click="showAddMemberModal = true" type="button" class="ml-3 inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            <svg class="-ml-1 mr-2 h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
              <path fill-rule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clip-rule="evenodd" />
            </svg>
            Add Member
          </button>
        </div>
      </div>
  
      <!-- Filters -->
      <div class="mt-8 bg-white shadow overflow-hidden sm:rounded-lg p-4">
        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
          <div>
            <label for="search" class="block text-sm font-medium text-gray-700">Search</label>
            <div class="mt-1 relative rounded-md shadow-sm">
              <input type="text" v-model="filters.search" id="search" class="focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" placeholder="Name, Email, Phone..." />
            </div>
          </div>
          <div>
            <label for="role" class="block text-sm font-medium text-gray-700">Role</label>
            <select id="role" v-model="filters.role" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
              <option value="">All Roles</option>
              <option value="member">Member</option>
              <option value="staff">Staff</option>
              <option value="admin">Administrator</option>
              <option value="pastor">Pastor</option>
              <option value="volunteer">Volunteer</option>
            </select>
          </div>
          <div>
            <label for="branch" class="block text-sm font-medium text-gray-700">Branch</label>
            <select id="branch" v-model="filters.branch" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
              <option value="">All Branches</option>
              <option value="main">Main Campus</option>
              <option value="north">North Campus</option>
              <option value="south">South Campus</option>
            </select>
          </div>
          <div>
            <label for="activity" class="block text-sm font-medium text-gray-700">Activity</label>
            <select id="activity" v-model="filters.activity" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
              <option value="">All Activity</option>
              <option value="active">Active</option>
              <option value="inactive">Inactive</option>
              <option value="new">New Members</option>
            </select>
          </div>
        </div>
      </div>
  
      <!-- Members Table -->
      <div class="mt-8 flex flex-col">
        <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
            <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
              <table class="min-w-full divide-y divide-gray-200">
                <thead class="bg-gray-50">
                  <tr>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                      Name
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                      Role
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                      Branch
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                      Status
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                      Last Activity
                    </th>
                    <th scope="col" class="relative px-6 py-3">
                      <span class="sr-only">Actions</span>
                    </th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="member in filteredMembers" :key="member.id">
                    <td class="px-6 py-4 whitespace-nowrap">
                      <div class="flex items-center">
                        <div class="flex-shrink-0 h-10 w-10">
                          <img class="h-10 w-10 rounded-full" :src="member.avatar" alt="" />
                        </div>
                        <div class="ml-4">
                          <div class="text-sm font-medium text-gray-900">
                            {{ member.name }}
                          </div>
                          <div class="text-sm text-gray-500">
                            {{ member.email }}
                          </div>
                        </div>
                      </div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <div class="text-sm text-gray-900">{{ member.role }}</div>
                      <div class="text-sm text-gray-500">{{ member.department }}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <div class="text-sm text-gray-900">{{ member.branch }}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap">
                      <span :class="[
                        'px-2 inline-flex text-xs leading-5 font-semibold rounded-full',
                        member.status === 'Active' ? 'bg-green-100 text-green-800' : 
                        member.status === 'Inactive' ? 'bg-red-100 text-red-800' : 
                        'bg-yellow-100 text-yellow-800'
                      ]">
                        {{ member.status }}
                      </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                      {{ member.lastActivity }}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                      <button @click="editMember(member)" class="text-indigo-600 hover:text-indigo-900 mr-4">Edit</button>
                      <button @click="viewMemberDetails(member)" class="text-gray-600 hover:text-gray-900">View</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Pagination -->
      <div class="bg-white px-4 py-3 flex items-center justify-between border-t border-gray-200 sm:px-6 mt-4">
        <div class="flex-1 flex justify-between sm:hidden">
          <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
            Previous
          </button>
          <button class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
            Next
          </button>
        </div>
        <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between">
          <div>
            <p class="text-sm text-gray-700">
              Showing <span class="font-medium">1</span> to <span class="font-medium">10</span> of <span class="font-medium">{{ members.length }}</span> results
            </p>
          </div>
          <div>
            <nav class="relative z-0 inline-flex rounded-md shadow-sm -space-x-px" aria-label="Pagination">
              <a href="#" class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                <span class="sr-only">Previous</span>
                <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
                </svg>
              </a>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                1
              </a>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                2
              </a>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-indigo-50 text-sm font-medium text-indigo-600 hover:bg-gray-50">
                3
              </a>
              <span class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700">
                ...
              </span>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                8
              </a>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                9
              </a>
              <a href="#" class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                10
              </a>
              <a href="#" class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                <span class="sr-only">Next</span>
                <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                </svg>
              </a>
            </nav>
          </div>
        </div>
      </div>
  
      <!-- Add Member Modal -->
      <div v-if="showAddMemberModal" class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true" @click="showAddMemberModal = false"></div>
  
          <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
  
          <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
            <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
              <div class="sm:flex sm:items-start">
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                  <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                    Add New Member
                  </h3>
                  <div class="mt-4 space-y-4">
                    <div>
                      <label for="member-name" class="block text-sm font-medium text-gray-700">Full Name</label>
                      <input type="text" v-model="newMember.name" id="member-name" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                    </div>
                    <div>
                      <label for="member-email" class="block text-sm font-medium text-gray-700">Email</label>
                      <input type="email" v-model="newMember.email" id="member-email" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                    </div>
                    <div>
                      <label for="member-phone" class="block text-sm font-medium text-gray-700">Phone</label>
                      <input type="tel" v-model="newMember.phone" id="member-phone" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" />
                    </div>
                    <div>
                      <label for="member-role" class="block text-sm font-medium text-gray-700">Role</label>
                      <select id="member-role" v-model="newMember.role" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
                        <option value="member">Member</option>
                        <option value="staff">Staff</option>
                        <option value="admin">Administrator</option>
                        <option value="pastor">Pastor</option>
                        <option value="volunteer">Volunteer</option>
                      </select>
                    </div>
                    <div>
                      <label for="member-branch" class="block text-sm font-medium text-gray-700">Branch</label>
                      <select id="member-branch" v-model="newMember.branch" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md">
                        <option value="Main Campus">Main Campus</option>
                        <option value="North Campus">North Campus</option>
                        <option value="South Campus">South Campus</option>
                      </select>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
              <button type="button" @click="addMember" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-indigo-600 text-base font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:ml-3 sm:w-auto sm:text-sm">
                Add Member
              </button>
              <button type="button" @click="showAddMemberModal = false" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
                Cancel
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, computed } from 'vue'
  
  const showAddMemberModal = ref(false)
  
  const filters = ref({
    search: '',
    role: '',
    branch: '',
    activity: ''
  })
  
  const members = ref([
    {
      id: 1,
      name: 'Jane Cooper',
      email: 'jane.cooper@example.com',
      avatar: '/placeholder.svg?height=40&width=40',
      role: 'Administrator',
      department: 'Finance',
      branch: 'Main Campus',
      status: 'Active',
      lastActivity: '2025-03-01',
      phone: '(555) 123-4567'
    },
    {
      id: 2,
      name: 'John Smith',
      email: 'john.smith@example.com',
      avatar: '/placeholder.svg?height=40&width=40',
      role: 'Pastor',
      department: 'Ministry',
      branch: 'North Campus',
      status: 'Active',
      lastActivity: '2025-03-02',
      phone: '(555) 234-5678'
    },
    {
      id: 3,
      name: 'Michael Johnson',
      email: 'michael.johnson@example.com',
      avatar: '/placeholder.svg?height=40&width=40',
      role: 'Staff',
      department: 'Youth Ministry',
      branch: 'Main Campus',
      status: 'Active',
      lastActivity: '2025-02-28',
      phone: '(555) 345-6789'
    },
    {
      id: 4,
      name: 'Sarah Williams',
      email: 'sarah.williams@example.com',
      avatar: '/placeholder.svg?height=40&width=40',
      role: 'Volunteer',
      department: 'Children\'s Ministry',
      branch: 'South Campus',
      status: 'Inactive',
      lastActivity: '2025-01-15',
      phone: '(555) 456-7890'
    },
    {
      id: 5,
      name: 'Robert Brown',
      email: 'robert.brown@example.com',
      avatar: '/placeholder.svg?height=40&width=40',
      role: 'Member',
      department: '',
      branch: 'Main Campus',
      status: 'New',
      lastActivity: '2025-03-05',
      phone: '(555) 567-8901'
    }
  ])
  
  const newMember = ref({
    name: '',
    email: '',
    phone: '',
    role: 'member',
    branch: 'Main Campus'
  })
  
  const filteredMembers = computed(() => {
    return members.value.filter(member => {
      const searchMatch = !filters.value.search || 
        member.name.toLowerCase().includes(filters.value.search.toLowerCase()) ||
        member.email.toLowerCase().includes(filters.value.search.toLowerCase())
      
      const roleMatch = !filters.value.role || 
        member.role.toLowerCase() === filters.value.role.toLowerCase()
      
      const branchMatch = !filters.value.branch || 
        member.branch.toLowerCase().includes(filters.value.branch.toLowerCase())
      
      const activityMatch = !filters.value.activity || 
        member.status.toLowerCase() === filters.value.activity.toLowerCase()
      
      return searchMatch && roleMatch && branchMatch && activityMatch
    })
  })
  
  const addMember = () => {
    const newId = Math.max(...members.value.map(m => m.id)) + 1
    
    members.value.push({
      id: newId,
      name: newMember.value.name,
      email: newMember.value.email,
      avatar: '/placeholder.svg?height=40&width=40',
      role: newMember.value.role.charAt(0).toUpperCase() + newMember.value.role.slice(1),
      department: '',
      branch: newMember.value.branch,
      status: 'New',
      lastActivity: new Date().toISOString().split('T')[0],
      phone: newMember.value.phone
    })
    
    // Reset form and close modal
    newMember.value = {
      name: '',
      email: '',
      phone: '',
      role: 'member',
      branch: 'Main Campus'
    }
    
    showAddMemberModal.value = false
  }
  
  definePageMeta({
    layout: 'dashboard'
  })
  
  
  const editMember = (member: any) => {
    // In a real app, this would open an edit modal or navigate to an edit page
    console.log('Edit member:', member)
  }
  
  const viewMemberDetails = (member: any) => {
    // In a real app, this would navigate to a member details page
    console.log('View member details:', member)
  }

  definePageMeta({
      layout: 'dashboard'
  })
  </script>