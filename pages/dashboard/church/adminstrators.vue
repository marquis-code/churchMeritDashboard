<template>
    <div>
      <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
        <h1 class="text-2xl font-semibold text-gray-800 dark:text-white">
          Assign Branch Administrators
        </h1>
        <button 
          @click="showCreateAdminModal = true"
          class="mt-4 md:mt-0 px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors flex items-center"
          v-motion
          :initial="{ opacity: 0, y: 10 }"
          :enter="{ opacity: 1, y: 0, transition: { delay: 200 } }"
        >
          <PlusIcon class="w-5 h-5 mr-2" />
          Add Administrator
        </button>
      </div>
  
      <!-- Administrator Search and Filters -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-4 mb-6"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 300 } }"
      >
        <div class="flex flex-col md:flex-row gap-4">
          <div class="flex-1">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Search Administrators</label>
            <div class="relative">
              <input 
                type="text" 
                v-model="searchQuery"
                class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                placeholder="Search by name, email, or position..."
              />
              <MagnifyingGlassIcon class="absolute left-3 top-2.5 w-5 h-5 text-gray-400" />
            </div>
          </div>
          <div class="w-full md:w-48">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Position</label>
            <select 
              v-model="positionFilter"
              class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            >
              <option value="">All Positions</option>
              <option value="Senior Administrator">Senior Administrator</option>
              <option value="Branch Manager">Branch Manager</option>
              <option value="Regional Director">Regional Director</option>
              <option value="Assistant Administrator">Assistant Administrator</option>
              <option value="Operations Manager">Operations Manager</option>
            </select>
          </div>
          <div class="w-full md:w-48">
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Branch Assignment</label>
            <select 
              v-model="branchFilter"
              class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            >
              <option value="">All Assignments</option>
              <option value="assigned">Assigned</option>
              <option value="unassigned">Unassigned</option>
            </select>
          </div>
        </div>
      </div>
  
      <!-- Administrators Table -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 400 } }"
      >
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="text-left text-gray-500 bg-gray-50 dark:bg-gray-700 dark:text-gray-300 border-b dark:border-gray-600">
                <th class="px-6 py-3 font-medium">Name</th>
                <th class="px-6 py-3 font-medium">Position</th>
                <th class="px-6 py-3 font-medium">Email</th>
                <th class="px-6 py-3 font-medium">Branch Assignment</th>
                <th class="px-6 py-3 font-medium">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr 
                v-for="(admin, index) in filteredAdministrators" 
                :key="admin.id"
                class="border-b dark:border-gray-600 hover:bg-gray-50 dark:hover:bg-gray-700"
                v-motion
                :initial="{ opacity: 0, y: 10 }"
                :enter="{ opacity: 1, y: 0, transition: { delay: 500 + (index * 50) } }"
              >
                <td class="px-6 py-4">
                  <div class="flex items-center">
                    <div class="w-10 h-10 flex-shrink-0 mr-3 bg-purple-100 rounded-full flex items-center justify-center">
                      <span class="text-purple-600 font-medium">{{ admin.name.charAt(0) }}</span>
                    </div>
                    <div>
                      <p class="font-medium text-gray-800 dark:text-white">{{ admin.name }}</p>
                      <p class="text-xs text-gray-500 dark:text-gray-400">ID: {{ admin.id }}</p>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ admin.position }}</td>
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ admin.email }}</td>
                <td class="px-6 py-4">
                  <div v-if="admin.assignedBranch">
                    <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200">
                      {{ admin.assignedBranch }}
                    </span>
                  </div>
                  <div v-else>
                    <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-gray-100 text-gray-800 dark:bg-gray-700 dark:text-gray-300">
                      Unassigned
                    </span>
                  </div>
                </td>
                <td class="px-6 py-4">
                  <div class="flex space-x-2">
                    <button 
                      @click="assignBranch(admin)"
                      class="p-1 text-purple-600 bg-purple-100 rounded-md hover:bg-purple-200 dark:bg-purple-900 dark:text-purple-300 dark:hover:bg-purple-800"
                      title="Assign Branch"
                    >
                      <BuildingOffice2Icon class="w-5 h-5" />
                    </button>
                    <button 
                      @click="editAdmin(admin)"
                      class="p-1 text-blue-600 bg-blue-100 rounded-md hover:bg-blue-200 dark:bg-blue-900 dark:text-blue-300 dark:hover:bg-blue-800"
                      title="Edit Administrator"
                    >
                      <PencilIcon class="w-5 h-5" />
                    </button>
                    <button 
                      @click="confirmDeleteAdmin(admin)"
                      class="p-1 text-red-600 bg-red-100 rounded-md hover:bg-red-200 dark:bg-red-900 dark:text-red-300 dark:hover:bg-red-800"
                      title="Delete Administrator"
                    >
                      <TrashIcon class="w-5 h-5" />
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        
        <!-- Empty State -->
        <div 
          v-if="filteredAdministrators.length === 0"
          class="py-8 text-center"
        >
          <UserIcon class="w-12 h-12 mx-auto text-gray-400 dark:text-gray-500 mb-3" />
          <h3 class="text-lg font-medium text-gray-800 dark:text-white mb-2">No administrators found</h3>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            {{ searchQuery || positionFilter || branchFilter ? 'No administrators match your search criteria.' : 'Get started by adding your first administrator.' }}
          </p>
          <button 
            @click="showCreateAdminModal = true"
            class="px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors inline-flex items-center"
          >
            <PlusIcon class="w-5 h-5 mr-2" />
            Add Administrator
          </button>
        </div>
      </div>
  
      <!-- Create/Edit Administrator Modal -->
      <Teleport to="body">
        <div v-if="showCreateAdminModal || showEditAdminModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
          <div 
            class="w-full max-w-md p-6 mx-4 bg-white rounded-lg shadow-xl dark:bg-gray-800"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :enter="{ opacity: 1, scale: 1 }"
          >
            <div class="flex items-center justify-between mb-4">
              <h3 class="text-lg font-semibold text-gray-800 dark:text-white">
                {{ showEditAdminModal ? 'Edit Administrator' : 'Add Administrator' }}
              </h3>
              <button 
                @click="closeModals"
                class="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
              >
                <XMarkIcon class="w-5 h-5" />
              </button>
            </div>
            <form @submit.prevent="showEditAdminModal ? updateAdmin() : createAdmin()">
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Full Name</label>
                <input 
                  type="text" 
                  v-model="adminForm.name"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Enter full name"
                  required
                />
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Email</label>
                <input 
                  type="email" 
                  v-model="adminForm.email"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Enter email address"
                  required
                />
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Position</label>
                <select 
                  v-model="adminForm.position"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="">Select Position</option>
                  <option value="Senior Administrator">Senior Administrator</option>
                  <option value="Branch Manager">Branch Manager</option>
                  <option value="Regional Director">Regional Director</option>
                  <option value="Assistant Administrator">Assistant Administrator</option>
                  <option value="Operations Manager">Operations Manager</option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Phone Number</label>
                <input 
                  type="tel" 
                  v-model="adminForm.phone"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Enter phone number"
                />
              </div>
              <div class="flex justify-end space-x-3">
                <button 
                  type="button"
                  @click="closeModals"
                  class="px-4 py-2 text-sm font-medium text-gray-700 transition-colors bg-gray-200 rounded-md hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600"
                >
                  Cancel
                </button>
                <button 
                  type="submit"
                  class="px-4 py-2 text-sm font-medium text-white transition-colors bg-purple-600 rounded-md hover:bg-purple-700"
                >
                  {{ showEditAdminModal ? 'Update Administrator' : 'Add Administrator' }}
                </button>
              </div>
            </form>
          </div>
        </div>
      </Teleport>
  
      <!-- Assign Branch Modal -->
      <Teleport to="body">
        <div v-if="showAssignBranchModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
          <div 
            class="w-full max-w-md p-6 mx-4 bg-white rounded-lg shadow-xl dark:bg-gray-800"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :enter="{ opacity: 1, scale: 1 }"
          >
            <div class="flex items-center justify-between mb-4">
              <h3 class="text-lg font-semibold text-gray-800 dark:text-white">
                Assign Branch to {{ selectedAdmin?.name }}
              </h3>
              <button 
                @click="showAssignBranchModal = false"
                class="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
              >
                <XMarkIcon class="w-5 h-5" />
              </button>
            </div>
            <form @submit.prevent="saveAssignment">
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Select Branch</label>
                <select 
                  v-model="assignmentForm.branchId"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="">Select Branch</option>
                  <option v-for="branch in availableBranches" :key="branch.id" :value="branch.id">
                    {{ branch.name }} ({{ branch.location }})
                  </option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Role at Branch</label>
                <input 
                  type="text" 
                  v-model="assignmentForm.role"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="e.g. Branch Administrator, Financial Officer"
                  required
                />
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Start Date</label>
                <input 
                  type="date" 
                  v-model="assignmentForm.startDate"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                />
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Notes</label>
                <textarea 
                  v-model="assignmentForm.notes"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Any additional notes about this assignment"
                  rows="3"
                ></textarea>
              </div>
              <div class="flex justify-end space-x-3">
                <button 
                  type="button"
                  @click="showAssignBranchModal = false"
                  class="px-4 py-2 text-sm font-medium text-gray-700 transition-colors bg-gray-200 rounded-md hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600"
                >
                  Cancel
                </button>
                <button 
                  type="submit"
                  class="px-4 py-2 text-sm font-medium text-white transition-colors bg-purple-600 rounded-md hover:bg-purple-700"
                >
                  Assign Branch
                </button>
              </div>
            </form>
          </div>
        </div>
      </Teleport>
  
      <!-- Delete Confirmation Modal -->
      <Teleport to="body">
        <div v-if="showDeleteConfirmModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
          <div 
            class="w-full max-w-md p-6 mx-4 bg-white rounded-lg shadow-xl dark:bg-gray-800"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :enter="{ opacity: 1, scale: 1 }"
          >
            <div class="text-center">
              <ExclamationTriangleIcon class="w-12 h-12 mx-auto text-red-500 mb-4" />
              <h3 class="text-lg font-semibold text-gray-800 dark:text-white mb-2">
                Delete Administrator
              </h3>
              <p class="text-gray-600 dark:text-gray-300 mb-6">
                Are you sure you want to delete <span class="font-medium">{{ selectedAdmin?.name }}</span>? This action cannot be undone.
              </p>
              <div class="flex justify-center space-x-3">
                <button 
                  @click="showDeleteConfirmModal = false"
                  class="px-4 py-2 text-sm font-medium text-gray-700 transition-colors bg-gray-200 rounded-md hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600"
                >
                  Cancel
                </button>
                <button 
                  @click="deleteAdmin"
                  class="px-4 py-2 text-sm font-medium text-white transition-colors bg-red-600 rounded-md hover:bg-red-700"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </Teleport>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, computed, onMounted } from 'vue';
  import { 
    PlusIcon, 
    UserIcon, 
    PencilIcon, 
    TrashIcon,
    XMarkIcon,
    MagnifyingGlassIcon,
    ExclamationTriangleIcon,
    BuildingOffice2Icon
  } from '@heroicons/vue/24/outline';
  import { useToast } from '@/composables/useToast';
  
  // Types
  interface Administrator {
    id: string;
    name: string;
    email: string;
    position: string;
    phone?: string;
    assignedBranch?: string;
    assignedBranchId?: string;
  }
  
  interface Branch {
    id: string;
    name: string;
    location: string;
    administratorId: string | null;
  }
  
  // State
  const administrators = ref<Administrator[]>([]);
  const branches = ref<Branch[]>([]);
  const searchQuery = ref('');
  const positionFilter = ref('');
  const branchFilter = ref('');
  const showCreateAdminModal = ref(false);
  const showEditAdminModal = ref(false);
  const showAssignBranchModal = ref(false);
  const showDeleteConfirmModal = ref(false);
  const selectedAdmin = ref<Administrator | null>(null);
  const adminForm = ref<Partial<Administrator>>({
    name: '',
    email: '',
    position: '',
    phone: ''
  });
  const assignmentForm = ref({
    branchId: '',
    role: '',
    startDate: '',
    notes: ''
  });
  
  const { showToast } = useToast();
  
  // Computed
  const filteredAdministrators = computed(() => {
    return administrators.value.filter(admin => {
      const matchesSearch = searchQuery.value === '' || 
        admin.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        admin.email.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        admin.position.toLowerCase().includes(searchQuery.value.toLowerCase());
      
      const matchesPosition = positionFilter.value === '' || admin.position === positionFilter.value;
      
      const matchesBranch = branchFilter.value === '' || 
        (branchFilter.value === 'assigned' && admin.assignedBranch) ||
        (branchFilter.value === 'unassigned' && !admin.assignedBranch);
      
      return matchesSearch && matchesPosition && matchesBranch;
    });
  });
  
  const availableBranches = computed(() => {
    // Filter out branches that already have administrators assigned
    // except for the branch currently assigned to the selected admin
    return branches.value.filter(branch => 
      !branch.administratorId || 
      (selectedAdmin.value && branch.administratorId === selectedAdmin.value.id)
    );
  });
  
  // Methods
  const fetchAdministrators = () => {
    // In a real app, this would be an API call
    // For now, we'll use dummy data
    administrators.value = [
      { 
        id: 'ADM001', 
        name: 'John Smith', 
        email: 'john.smith@example.com', 
        position: 'Senior Administrator',
        phone: '(555) 123-4567',
        assignedBranch: 'Main Cathedral',
        assignedBranchId: 'BR001'
      },
      { 
        id: 'ADM002', 
        name: 'Emily Davis', 
        email: 'emily.davis@example.com', 
        position: 'Branch Manager',
        phone: '(555) 234-5678',
        assignedBranch: 'Westside Chapel',
        assignedBranchId: 'BR002'
      },
      { 
        id: 'ADM003', 
        name: 'David Martinez', 
        email: 'david.martinez@example.com', 
        position: 'Regional Director',
        phone: '(555) 345-6789',
        assignedBranch: 'Southside Community Church',
        assignedBranchId: 'BR003'
      },
      { 
        id: 'ADM004', 
        name: 'Lisa Johnson', 
        email: 'lisa.johnson@example.com', 
        position: 'Assistant Administrator',
        phone: '(555) 456-7890',
        assignedBranch: 'Central City Church',
        assignedBranchId: 'BR005'
      },
      { 
        id: 'ADM005', 
        name: 'Michael Wilson', 
        email: 'michael.wilson@example.com', 
        position: 'Operations Manager',
        phone: '(555) 567-8901'
      }
    ];
  };
  
  const fetchBranches = () => {
    // In a real app, this would be an API call
    branches.value = [
      { id: 'BR001', name: 'Main Cathedral', location: 'New York', administratorId: 'ADM001' },
      { id: 'BR002', name: 'Westside Chapel', location: 'Los Angeles', administratorId: 'ADM002' },
      { id: 'BR003', name: 'Southside Community Church', location: 'Miami', administratorId: 'ADM003' },
      { id: 'BR004', name: 'Eastside Fellowship', location: 'Boston', administratorId: null },
      { id: 'BR005', name: 'Central City Church', location: 'Chicago', administratorId: 'ADM004' }
    ];
  };
  
  const createAdmin = () => {
    // In a real app, this would be an API call
    const newAdmin: Administrator = {
      id: `ADM${String(administrators.value.length + 1).padStart(3, '0')}`,
      name: adminForm.value.name || '',
      email: adminForm.value.email || '',
      position: adminForm.value.position || '',
      phone: adminForm.value.phone
    };
    
    administrators.value.push(newAdmin);
    showToast('Administrator added successfully!', 'success');
    closeModals();
  };
  
  const updateAdmin = () => {
    if (!selectedAdmin.value) return;
    
    // In a real app, this would be an API call
    const index = administrators.value.findIndex(a => a.id === selectedAdmin.value?.id);
    if (index !== -1) {
      administrators.value[index] = {
        ...administrators.value[index],
        name: adminForm.value.name || administrators.value[index].name,
        email: adminForm.value.email || administrators.value[index].email,
        position: adminForm.value.position || administrators.value[index].position,
        phone: adminForm.value.phone
      };
      
      showToast('Administrator updated successfully!', 'success');
      closeModals();
    }
  };
  
  const deleteAdmin = () => {
    if (!selectedAdmin.value) return;
    
    // In a real app, this would be an API call
    administrators.value = administrators.value.filter(a => a.id !== selectedAdmin.value?.id);
    
    // Also update any branches that had this admin assigned
    branches.value = branches.value.map(branch => {
      if (branch.administratorId === selectedAdmin.value?.id) {
        return { ...branch, administratorId: null };
      }
      return branch;
    });
    
    showToast('Administrator deleted successfully!', 'success');
    showDeleteConfirmModal.value = false;
  };
  
  const assignBranch = (admin: Administrator) => {
    selectedAdmin.value = admin;
    
    // Pre-fill the form if admin already has a branch assigned
    if (admin.assignedBranchId) {
      assignmentForm.branchId = admin.assignedBranchId;
      assignmentForm.role = 'Branch Administrator'; // Default role
      assignmentForm.startDate = new Date().toISOString().split('T')[0]; // Today's date
      assignmentForm.notes = '';
    } else {
      assignmentForm.branchId = '';
      assignmentForm.role = 'Branch Administrator';
      assignmentForm.startDate = new Date().toISOString().split('T')[0];
      assignmentForm.notes = '';
    }
    
    showAssignBranchModal.value = true;
  };
  
  const saveAssignment = () => {
    if (!selectedAdmin.value) return;
    
    // In a real app, this would be an API call
    const adminIndex = administrators.value.findIndex(a => a.id === selectedAdmin.value?.id);
    
    if (adminIndex !== -1) {
      // If admin was previously assigned to a branch, update that branch
      if (administrators.value[adminIndex].assignedBranchId) {
        const oldBranchIndex = branches.value.findIndex(b => b.id === administrators.value[adminIndex].assignedBranchId);
        if (oldBranchIndex !== -1) {
          branches.value[oldBranchIndex].administratorId = null;
        }
      }
      
      // Update the branch with the new admin
      const branchIndex = branches.value.findIndex(b => b.id === assignmentForm.branchId);
      if (branchIndex !== -1) {
        branches.value[branchIndex].administratorId = selectedAdmin.value.id;
        
        // Update the admin with the new branch
        administrators.value[adminIndex].assignedBranchId = assignmentForm.branchId;
        administrators.value[adminIndex].assignedBranch = branches.value[branchIndex].name;
      }
      
      showToast('Branch assignment updated successfully!', 'success');
      showAssignBranchModal.value = false;
    }
  };
  
  const editAdmin = (admin: Administrator) => {
    selectedAdmin.value = admin;
    adminForm.value = {
      name: admin.name,
      email: admin.email,
      position: admin.position,
      phone: admin.phone
    };
    showEditAdminModal.value = true;
  };
  
  const confirmDeleteAdmin = (admin: Administrator) => {
    selectedAdmin.value = admin;
    showDeleteConfirmModal.value = true;
  };
  
  const closeModals = () => {
    showCreateAdminModal.value = false;
    showEditAdminModal.value = false;
    resetForm();
  };
  
  const resetForm = () => {
    adminForm.value = {
      name: '',
      email: '',
      position: '',
      phone: ''
    };
    selectedAdmin.value = null;
  };
  
  onMounted(() => {
    fetchAdministrators();
    fetchBranches();
  });
  </script>