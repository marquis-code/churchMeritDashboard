<template>
    <div>
      <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
        <h1 class="text-2xl font-semibold text-gray-800 dark:text-white">
          Set Reporting Lines
        </h1>
        <button 
          @click="showCreateReportingLineModal = true"
          class="mt-4 md:mt-0 px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors flex items-center"
          v-motion
          :initial="{ opacity: 0, y: 10 }"
          :enter="{ opacity: 1, y: 0, transition: { delay: 200 } }"
        >
          <PlusIcon class="w-5 h-5 mr-2" />
          Create Reporting Line
        </button>
      </div>
  
      <!-- Organization Chart -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-6"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 300 } }"
      >
        <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Organization Chart</h2>
        <div class="overflow-x-auto">
          <div class="min-w-[800px] h-[400px] relative">
            <OrganizationChart :data="orgChartData" />
          </div>
        </div>
      </div>
  
      <!-- Reporting Lines Table -->
      <div 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { delay: 400 } }"
      >
        <div class="p-4 border-b dark:border-gray-700 flex flex-col md:flex-row md:items-center md:justify-between">
          <h2 class="text-lg font-semibold text-gray-800 dark:text-white">Reporting Lines</h2>
          <div class="mt-3 md:mt-0 flex items-center">
            <div class="relative">
              <input 
                type="text" 
                v-model="searchQuery"
                class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                placeholder="Search reporting lines..."
              />
              <MagnifyingGlassIcon class="absolute left-3 top-2.5 w-5 h-5 text-gray-400" />
            </div>
            <select 
              v-model="branchFilter"
              class="ml-2 px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
            >
              <option value="">All Branches</option>
              <option v-for="branch in branches" :key="branch.id" :value="branch.id">
                {{ branch.name }}
              </option>
            </select>
          </div>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="text-left text-gray-500 bg-gray-50 dark:bg-gray-700 dark:text-gray-300 border-b dark:border-gray-600">
                <th class="px-6 py-3 font-medium">Employee</th>
                <th class="px-6 py-3 font-medium">Position</th>
                <th class="px-6 py-3 font-medium">Reports To</th>
                <th class="px-6 py-3 font-medium">Branch</th>
                <th class="px-6 py-3 font-medium">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr 
                v-for="(line, index) in filteredReportingLines" 
                :key="index"
                class="border-b dark:border-gray-600 hover:bg-gray-50 dark:hover:bg-gray-700"
                v-motion
                :initial="{ opacity: 0, y: 10 }"
                :enter="{ opacity: 1, y: 0, transition: { delay: 500 + (index * 50) } }"
              >
                <td class="px-6 py-4">
                  <div class="flex items-center">
                    <div class="w-8 h-8 flex-shrink-0 mr-3 bg-purple-100 rounded-full flex items-center justify-center">
                      <span class="text-purple-600 font-medium">{{ line.employeeName.charAt(0) }}</span>
                    </div>
                    <div>
                      <p class="font-medium text-gray-800 dark:text-white">{{ line.employeeName }}</p>
                      <p class="text-xs text-gray-500 dark:text-gray-400">ID: {{ line.employeeId }}</p>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4 text-gray-800 dark:text-gray-200">{{ line.position }}</td>
                <td class="px-6 py-4">
                  <div class="flex items-center">
                    <div class="w-8 h-8 flex-shrink-0 mr-3 bg-blue-100 rounded-full flex items-center justify-center">
                      <span class="text-blue-600 font-medium">{{ line.reportsToName.charAt(0) }}</span>
                    </div>
                    <div>
                      <p class="font-medium text-gray-800 dark:text-white">{{ line.reportsToName }}</p>
                      <p class="text-xs text-gray-500 dark:text-gray-400">{{ line.reportsToPosition }}</p>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4">
                  <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-purple-100 text-purple-800 dark:bg-purple-900 dark:text-purple-200">
                    {{ line.branchName }}
                  </span>
                </td>
                <td class="px-6 py-4">
                  <div class="flex space-x-2">
                    <button 
                      @click="editReportingLine(line)"
                      class="p-1 text-blue-600 bg-blue-100 rounded-md hover:bg-blue-200 dark:bg-blue-900 dark:text-blue-300 dark:hover:bg-blue-800"
                      title="Edit Reporting Line"
                    >
                      <PencilIcon class="w-5 h-5" />
                    </button>
                    <button 
                      @click="confirmDeleteReportingLine(line)"
                      class="p-1 text-red-600 bg-red-100 rounded-md hover:bg-red-200 dark:bg-red-900 dark:text-red-300 dark:hover:bg-red-800"
                      title="Delete Reporting Line"
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
          v-if="filteredReportingLines.length === 0"
          class="py-8 text-center"
        >
          <UsersIcon class="w-12 h-12 mx-auto text-gray-400 dark:text-gray-500 mb-3" />
          <h3 class="text-lg font-medium text-gray-800 dark:text-white mb-2">No reporting lines found</h3>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            {{ searchQuery || branchFilter ? 'No reporting lines match your search criteria.' : 'Get started by creating your first reporting line.' }}
          </p>
          <button 
            @click="showCreateReportingLineModal = true"
            class="px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700 transition-colors inline-flex items-center"
          >
            <PlusIcon class="w-5 h-5 mr-2" />
            Create Reporting Line
          </button>
        </div>
      </div>
  
      <!-- Create/Edit Reporting Line Modal -->
      <Teleport to="body">
        <div v-if="showCreateReportingLineModal || showEditReportingLineModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
          <div 
            class="w-full max-w-md p-6 mx-4 bg-white rounded-lg shadow-xl dark:bg-gray-800"
            v-motion
            :initial="{ opacity: 0, scale: 0.9 }"
            :enter="{ opacity: 1, scale: 1 }"
          >
            <div class="flex items-center justify-between mb-4">
              <h3 class="text-lg font-semibold text-gray-800 dark:text-white">
                {{ showEditReportingLineModal ? 'Edit Reporting Line' : 'Create Reporting Line' }}
              </h3>
              <button 
                @click="closeModals"
                class="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200"
              >
                <XMarkIcon class="w-5 h-5" />
              </button>
            </div>
            <form @submit.prevent="showEditReportingLineModal ? updateReportingLine() : createReportingLine()">
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Branch</label>
                <select 
                  v-model="reportingLineForm.branchId"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                  @change="loadBranchEmployees"
                >
                  <option value="">Select Branch</option>
                  <option v-for="branch in branches" :key="branch.id" :value="branch.id">
                    {{ branch.name }}
                  </option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Employee</label>
                <select 
                  v-model="reportingLineForm.employeeId"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="">Select Employee</option>
                  <option v-for="employee in branchEmployees" :key="employee.id" :value="employee.id">
                    {{ employee.name }} ({{ employee.position }})
                  </option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Reports To</label>
                <select 
                  v-model="reportingLineForm.reportsToId"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="">Select Supervisor</option>
                  <option v-for="supervisor in availableSupervisors" :key="supervisor.id" :value="supervisor.id">
                    {{ supervisor.name }} ({{ supervisor.position }})
                  </option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Relationship Type</label>
                <select 
                  v-model="reportingLineForm.relationshipType"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  required
                >
                  <option value="">Select Relationship Type</option>
                  <option value="Direct">Direct Report</option>
                  <option value="Functional">Functional Report</option>
                  <option value="Dotted Line">Dotted Line</option>
                  <option value="Matrix">Matrix Reporting</option>
                </select>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-700 dark:text-gray-300">Notes</label>
                <textarea 
                  v-model="reportingLineForm.notes"
                  class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500 dark:bg-gray-700 dark:border-gray-600 dark:text-white"
                  placeholder="Any additional notes about this reporting relationship"
                  rows="3"
                ></textarea>
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
                  {{ showEditReportingLineModal ? 'Update Reporting Line' : 'Create Reporting Line' }}
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
                Delete Reporting Line
              </h3>
              <p class="text-gray-600 dark:text-gray-300 mb-6">
                Are you sure you want to delete the reporting line between 
                <span class="font-medium">{{ selectedReportingLine?.employeeName }}</span> and 
                <span class="font-medium">{{ selectedReportingLine?.reportsToName }}</span>? 
                This action cannot be undone.
              </p>
              <div class="flex justify-center space-x-3">
                <button 
                  @click="showDeleteConfirmModal = false"
                  class="px-4 py-2 text-sm font-medium text-gray-700 transition-colors bg-gray-200 rounded-md hover:bg-gray-300 dark:bg-gray-700 dark:text-gray-200 dark:hover:bg-gray-600"
                >
                  Cancel
                </button>
                <button 
                  @click="deleteReportingLine"
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
    UsersIcon, 
    PencilIcon, 
    TrashIcon,
    XMarkIcon,
    MagnifyingGlassIcon,
    ExclamationTriangleIcon
  } from '@heroicons/vue/24/outline';
  import { useToast } from '@/composables/useToast';
  
  // Types
  interface Branch {
    id: string;
    name: string;
    location: string;
  }
  
  interface Employee {
    id: string;
    name: string;
    position: string;
    branchId: string;
    isSupervisor?: boolean;
  }
  
  interface ReportingLine {
    id: string;
    employeeId: string;
    employeeName: string;
    position: string;
    reportsToId: string;
    reportsToName: string;
    reportsToPosition: string;
    branchId: string;
    branchName: string;
    relationshipType: string;
    notes?: string;
  }
  
  // State
  const branches = ref<Branch[]>([]);
  const employees = ref<Employee[]>([]);
  const reportingLines = ref<ReportingLine[]>([]);
  const branchEmployees = ref<Employee[]>([]);
  const availableSupervisors = ref<Employee[]>([]);
  const searchQuery = ref('');
  const branchFilter = ref('');
  const showCreateReportingLineModal = ref(false);
  const showEditReportingLineModal = ref(false);
  const showDeleteConfirmModal = ref(false);
  const selectedReportingLine = ref<ReportingLine | null>(null);
  const reportingLineForm = ref({
    branchId: '',
    employeeId: '',
    reportsToId: '',
    relationshipType: '',
    notes: ''
  });
  
  const { showToast } = useToast();
  
  // Computed
  const filteredReportingLines = computed(() => {
    return reportingLines.value.filter(line => {
      const matchesSearch = searchQuery.value === '' || 
        line.employeeName.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        line.reportsToName.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        line.position.toLowerCase().includes(searchQuery.value.toLowerCase());
      
      const matchesBranch = branchFilter.value === '' || line.branchId === branchFilter.value;
      
      return matchesSearch && matchesBranch;
    });
  });
  
  // Organization Chart Data
  const orgChartData = computed(() => {
    // Create a hierarchical structure for the org chart
    const rootNodes: any[] = [];
    const nodeMap = new Map();
    
    // First, create all nodes
    employees.value.forEach(employee => {
      nodeMap.set(employee.id, {
        id: employee.id,
        name: employee.name,
        title: employee.position,
        children: []
      });
    });
    
    // Then, establish the hierarchy based on reporting lines
    reportingLines.value.forEach(line => {
      const employeeNode = nodeMap.get(line.employeeId);
      const supervisorNode = nodeMap.get(line.reportsToId);
      
      if (employeeNode && supervisorNode) {
        supervisorNode.children.push(employeeNode);
      }
    });
    
    // Find root nodes (employees who don't report to anyone in our system)
    const reportingEmployeeIds = new Set(reportingLines.value.map(line => line.employeeId));
    
    employees.value.forEach(employee => {
      if (!reportingEmployeeIds.has(employee.id)) {
        const node = nodeMap.get(employee.id);
        if (node) {
          rootNodes.push(node);
        }
      }
    });
    
    // If there are multiple root nodes, create a virtual root
    if (rootNodes.length > 1) {
      return {
        id: 'root',
        name: 'Organization',
        title: 'Church Leadership',
        children: rootNodes
      };
    } else if (rootNodes.length === 1) {
      return rootNodes[0];
    } else {
      return {
        id: 'empty',
        name: 'No Data',
        title: 'Please add reporting lines',
        children: []
      };
    }
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
  
  const fetchEmployees = () => {
    // In a real app, this would be an API call
    employees.value = [
      { id: 'EMP001', name: 'John Smith', position: 'Senior Pastor', branchId: 'BR001', isSupervisor: true },
      { id: 'EMP002', name: 'Emily Davis', position: 'Branch Manager', branchId: 'BR002', isSupervisor: true },
      { id: 'EMP003', name: 'David Martinez', position: 'Regional Director', branchId: 'BR003', isSupervisor: true },
      { id: 'EMP004', name: 'Lisa Johnson', position: 'Assistant Administrator', branchId: 'BR005', isSupervisor: true },
      { id: 'EMP005', name: 'Michael Wilson', position: 'Operations Manager', branchId: 'BR001', isSupervisor: true },
      { id: 'EMP006', name: 'Sarah Thompson', position: 'Youth Pastor', branchId: 'BR001', isSupervisor: false },
      { id: 'EMP007', name: 'Robert Brown', position: 'Worship Leader', branchId: 'BR002', isSupervisor: false },
      { id: 'EMP008', name: 'Jennifer Lee', position: 'Children\'s Ministry Director', branchId: 'BR003', isSupervisor: true },
      { id: 'EMP009', name: 'Daniel Clark', position: 'Financial Officer', branchId: 'BR001', isSupervisor: false },
      { id: 'EMP010', name: 'Amanda White', position: 'Community Outreach Coordinator', branchId: 'BR002', isSupervisor: false },
      { id: 'EMP011', name: 'James Wilson', position: 'Facilities Manager', branchId: 'BR001', isSupervisor: false },
      { id: 'EMP012', name: 'Patricia Moore', position: 'Administrative Assistant', branchId: 'BR003', isSupervisor: false }
    ];
  };
  
  const fetchReportingLines = () => {
    // In a real app, this would be an API call
    reportingLines.value = [
      {
        id: 'RL001',
        employeeId: 'EMP005',
        employeeName: 'Michael Wilson',
        position: 'Operations Manager',
        reportsToId: 'EMP001',
        reportsToName: 'John Smith',
        reportsToPosition: 'Senior Pastor',
        branchId: 'BR001',
        branchName: 'Main Cathedral',
        relationshipType: 'Direct'
      },
      {
        id: 'RL002',
        employeeId: 'EMP006',
        employeeName: 'Sarah Thompson',
        position: 'Youth Pastor',
        reportsToId: 'EMP001',
        reportsToName: 'John Smith',
        reportsToPosition: 'Senior Pastor',
        branchId: 'BR001',
        branchName: 'Main Cathedral',
        relationshipType: 'Direct'
      },
      {
        id: 'RL003',
        employeeId: 'EMP007',
        employeeName: 'Robert Brown',
        position: 'Worship Leader',
        reportsToId: 'EMP002',
        reportsToName: 'Emily Davis',
        reportsToPosition: 'Branch Manager',
        branchId: 'BR002',
        branchName: 'Westside Chapel',
        relationshipType: 'Direct'
      },
      {
        id: 'RL004',
        employeeId: 'EMP009',
        employeeName: 'Daniel Clark',
        position: 'Financial Officer',
        reportsToId: 'EMP005',
        reportsToName: 'Michael Wilson',
        reportsToPosition: 'Operations Manager',
        branchId: 'BR001',
        branchName: 'Main Cathedral',
        relationshipType: 'Direct'
      },
      {
        id: 'RL005',
        employeeId: 'EMP010',
        employeeName: 'Amanda White',
        position: 'Community Outreach Coordinator',
        reportsToId: 'EMP002',
        reportsToName: 'Emily Davis',
        reportsToPosition: 'Branch Manager',
        branchId: 'BR002',
        branchName: 'Westside Chapel',
        relationshipType: 'Direct'
      },
      {
        id: 'RL006',
        employeeId: 'EMP011',
        employeeName: 'James Wilson',
        position: 'Facilities Manager',
        reportsToId: 'EMP005',
        reportsToName: 'Michael Wilson',
        reportsToPosition: 'Operations Manager',
        branchId: 'BR001',
        branchName: 'Main Cathedral',
        relationshipType: 'Direct'
      },
      {
        id: 'RL007',
        employeeId: 'EMP012',
        employeeName: 'Patricia Moore',
        position: 'Administrative Assistant',
        reportsToId: 'EMP003',
        reportsToName: 'David Martinez',
        reportsToPosition: 'Regional Director',
        branchId: 'BR003',
        branchName: 'Southside Community Church',
        relationshipType: 'Direct'
      }
    ];
  };
  
  const loadBranchEmployees = () => {
    if (!reportingLineForm.value.branchId) {
      branchEmployees.value = [];
      availableSupervisors.value = [];
      return;
    }
    
    // Filter employees by selected branch
    branchEmployees.value = employees.value.filter(emp => emp.branchId === reportingLineForm.value.branchId);
    
    // Filter supervisors (could be from any branch for cross-branch reporting)
    availableSupervisors.value = employees.value.filter(emp => emp.isSupervisor);
  };
  
  const createReportingLine = () => {
    // In a real app, this would be an API call
    const employee = employees.value.find(emp => emp.id === reportingLineForm.value.employeeId);
    const supervisor = employees.value.find(emp => emp.id === reportingLineForm.value.reportsToId);
    const branch = branches.value.find(b => b.id === reportingLineForm.value.branchId);
    
    if (!employee || !supervisor || !branch) return;
    
    const newReportingLine: ReportingLine = {
      id: `RL${String(reportingLines.value.length + 1).padStart(3, '0')}`,
      employeeId: employee.id,
      employeeName: employee.name,
      position: employee.position,
      reportsToId: supervisor.id,
      reportsToName: supervisor.name,
      reportsToPosition: supervisor.position,
      branchId: branch.id,
      branchName: branch.name,
      relationshipType: reportingLineForm.value.relationshipType,
      notes: reportingLineForm.value.notes
    };
    
    reportingLines.value.push(newReportingLine);
    showToast('Reporting line created successfully!', 'success');
    closeModals();
  };
  
  const updateReportingLine = () => {
    if (!selectedReportingLine.value) return;
    
    // In a real app, this would be an API call
    const employee = employees.value.find(emp => emp.id === reportingLineForm.value.employeeId);
    const supervisor = employees.value.find(emp => emp.id === reportingLineForm.value.reportsToId);
    const branch = branches.value.find(b => b.id === reportingLineForm.value.branchId);
    
    if (!employee || !supervisor || !branch) return;
    
    const index = reportingLines.value.findIndex(line => line.id === selectedReportingLine.value?.id);
    if (index !== -1) {
      reportingLines.value[index] = {
        ...reportingLines.value[index],
        employeeId: employee.id,
        employeeName: employee.name,
        position: employee.position,
        reportsToId: supervisor.id,
        reportsToName: supervisor.name,
        reportsToPosition: supervisor.position,
        branchId: branch.id,
        branchName: branch.name,
        relationshipType: reportingLineForm.value.relationshipType,
        notes: reportingLineForm.value.notes
      };
      
      showToast('Reporting line updated successfully!', 'success');
      closeModals();
    }
  };
  
  const deleteReportingLine = () => {
    if (!selectedReportingLine.value) return;
    
    // In a real app, this would be an API call
    reportingLines.value = reportingLines.value.filter(line => line.id !== selectedReportingLine.value?.id);
    
    showToast('Reporting line deleted successfully!', 'success');
    showDeleteConfirmModal.value = false;
  };
  
  const editReportingLine = (line: ReportingLine) => {
    selectedReportingLine.value = line;
    reportingLineForm.value = {
      branchId: line.branchId,
      employeeId: line.employeeId,
      reportsToId: line.reportsToId,
      relationshipType: line.relationshipType,
      notes: line.notes || ''
    };
    
    // Load branch employees
    loadBranchEmployees();
    
    showEditReportingLineModal.value = true;
  };
  
  const confirmDeleteReportingLine = (line: ReportingLine) => {
    selectedReportingLine.value = line;
    showDeleteConfirmModal.value = true;
  };
  
  const closeModals = () => {
    showCreateReportingLineModal.value = false;
    showEditReportingLineModal.value = false;
    resetForm();
  };
  
  const resetForm = () => {
    reportingLineForm.value = {
      branchId: '',
      employeeId: '',
      reportsToId: '',
      relationshipType: '',
      notes: ''
    };
    selectedReportingLine.value = null;
    branchEmployees.value = [];
    availableSupervisors.value = [];
  };
  
  onMounted(() => {
    fetchBranches();
    fetchEmployees();
    fetchReportingLines();
  });
  </script>