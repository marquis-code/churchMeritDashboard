<template>
    <div>
      <div class="mb-6 flex items-center justify-between">
        <h2 class="text-2xl font-bold text-gray-900 dark:text-white">Volunteer Assignments</h2>
        <div class="flex space-x-3">
          <button
            @click="showVolunteerModal = true"
            class="inline-flex items-center rounded-md bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 dark:bg-gray-800 dark:text-gray-300 dark:ring-gray-600 dark:hover:bg-gray-700"
          >
            <UserPlusIcon class="mr-2 h-4 w-4" />
            Add Volunteer
          </button>
          <button
            @click="showRoleModal = true"
            class="inline-flex items-center rounded-md bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
          >
            <PlusIcon class="mr-2 h-4 w-4" />
            Create Role
          </button>
        </div>
      </div>
  
      <!-- Departments and roles -->
      <div class="mb-8 grid gap-6 md:grid-cols-2 lg:grid-cols-3">
        <div v-for="(department, index) in departments" :key="index" class="rounded-lg border border-gray-200 bg-white shadow-sm dark:border-gray-700 dark:bg-gray-800">
          <div class="border-b border-gray-200 px-4 py-3 dark:border-gray-700">
            <div class="flex items-center justify-between">
              <h3 class="text-base font-medium text-gray-900 dark:text-white">{{ department.name }}</h3>
              <div class="flex items-center space-x-2">
                <button class="rounded-md p-1 text-gray-400 hover:bg-gray-100 hover:text-gray-500 dark:hover:bg-gray-700 dark:hover:text-gray-300">
                  <PencilIcon class="h-4 w-4" />
                </button>
                <button class="rounded-md p-1 text-gray-400 hover:bg-gray-100 hover:text-gray-500 dark:hover:bg-gray-700 dark:hover:text-gray-300">
                  <DotsVerticalIcon class="h-4 w-4" />
                </button>
              </div>
            </div>
            <p class="text-xs text-gray-500 dark:text-gray-400">{{ department.description }}</p>
          </div>
          <div class="p-4">
            <ul class="space-y-3">
              <li v-for="(role, roleIndex) in department.roles" :key="roleIndex" class="rounded-md border border-gray-200 p-3 dark:border-gray-700">
                <div class="flex items-center justify-between">
                  <div>
                    <h4 class="text-sm font-medium text-gray-900 dark:text-white">{{ role.name }}</h4>
                    <div class="mt-1 flex items-center text-xs text-gray-500 dark:text-gray-400">
                      <UsersIcon class="mr-1 h-3 w-3" />
                      {{ role.assigned }}/{{ role.needed }} assigned
                    </div>
                  </div>
                  <button
                    class="rounded-md bg-purple-50 px-2 py-1 text-xs font-medium text-purple-700 hover:bg-purple-100 dark:bg-purple-900/20 dark:text-purple-300 dark:hover:bg-purple-900/30"
                    @click="openAssignVolunteers(department, role)"
                  >
                    Assign
                  </button>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
  
      <!-- Volunteer roster -->
      <div class="rounded-lg border border-gray-200 bg-white shadow-sm dark:border-gray-700 dark:bg-gray-800">
        <div class="border-b border-gray-200 px-6 py-4 dark:border-gray-700">
          <div class="flex items-center justify-between">
            <h3 class="text-lg font-medium text-gray-900 dark:text-white">Volunteer Roster</h3>
            <div class="flex items-center space-x-2">
              <div class="relative">
                <SearchIcon class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-gray-400 dark:text-gray-500" />
                <input
                  type="text"
                  placeholder="Search volunteers..."
                  class="h-9 rounded-md border border-gray-300 bg-white pl-10 pr-4 text-sm text-gray-900 placeholder-gray-500 focus:border-purple-500 focus:outline-none focus:ring-1 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                />
              </div>
              <select
                class="h-9 rounded-md border border-gray-300 bg-white px-3 text-sm text-gray-900 focus:border-purple-500 focus:outline-none focus:ring-1 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
              >
                <option>All Departments</option>
                <option>Worship</option>
                <option>Children's Ministry</option>
                <option>Hospitality</option>
                <option>Technical</option>
                <option>Outreach</option>
              </select>
            </div>
          </div>
        </div>
        <div class="p-6">
          <div class="overflow-hidden">
            <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
              <thead class="bg-gray-50 dark:bg-gray-800/50">
                <tr>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Name
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Roles
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Contact
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Status
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Last Served
                  </th>
                  <th scope="col" class="px-6 py-3 text-right text-xs font-medium uppercase tracking-wider text-gray-500 dark:text-gray-400">
                    Actions
                  </th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200 bg-white dark:divide-gray-700 dark:bg-gray-800">
                <tr v-for="(volunteer, index) in volunteers" :key="index" class="hover:bg-gray-50 dark:hover:bg-gray-700/50">
                  <td class="whitespace-nowrap px-6 py-4">
                    <div class="flex items-center">
                      <div class="h-10 w-10 flex-shrink-0">
                        <img class="h-10 w-10 rounded-full" :src="volunteer.avatar" alt="" />
                      </div>
                      <div class="ml-4">
                        <div class="text-sm font-medium text-gray-900 dark:text-white">{{ volunteer.name }}</div>
                        <div class="text-xs text-gray-500 dark:text-gray-400">Member since {{ volunteer.memberSince }}</div>
                      </div>
                    </div>
                  </td>
                  <td class="whitespace-nowrap px-6 py-4">
                    <div class="flex flex-wrap gap-1">
                      <span
                        v-for="(role, roleIndex) in volunteer.roles"
                        :key="roleIndex"
                        class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium"
                        :class="getRoleBadgeClass(role)"
                      >
                        {{ role }}
                      </span>
                    </div>
                  </td>
                  <td class="whitespace-nowrap px-6 py-4 text-sm text-gray-500 dark:text-gray-400">
                    <div>{{ volunteer.email }}</div>
                    <div>{{ volunteer.phone }}</div>
                  </td>
                  <td class="whitespace-nowrap px-6 py-4">
                    <span
                      class="inline-flex rounded-full px-2 text-xs font-semibold leading-5"
                      :class="[
                        volunteer.status === 'Active'
                          ? 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300'
                          : volunteer.status === 'On Leave'
                          ? 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300'
                          : 'bg-gray-100 text-gray-800 dark:bg-gray-700 dark:text-gray-300'
                      ]"
                    >
                      {{ volunteer.status }}
                    </span>
                  </td>
                  <td class="whitespace-nowrap px-6 py-4 text-sm text-gray-500 dark:text-gray-400">
                    {{ volunteer.lastServed }}
                  </td>
                  <td class="whitespace-nowrap px-6 py-4 text-right text-sm font-medium">
                    <button class="text-purple-600 hover:text-purple-900 dark:text-purple-400 dark:hover:text-purple-300">
                      Schedule
                    </button>
                    <button class="ml-3 text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-300">
                      Edit
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="mt-4 flex items-center justify-between">
            <div class="text-sm text-gray-700 dark:text-gray-300">
              Showing <span class="font-medium">1</span> to <span class="font-medium">10</span> of <span class="font-medium">24</span> volunteers
            </div>
            <div class="flex space-x-2">
              <button class="inline-flex items-center rounded-md border border-gray-300 bg-white px-3 py-1.5 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600">
                Previous
              </button>
              <button class="inline-flex items-center rounded-md border border-gray-300 bg-white px-3 py-1.5 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600">
                Next
              </button>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Add Volunteer Modal -->
      <Teleport to="body">
        <div v-if="showVolunteerModal" class="fixed inset-0 z-50 overflow-y-auto">
          <div class="flex min-h-screen items-end justify-center px-4 pt-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="showVolunteerModal = false"></div>
            <span class="hidden sm:inline-block sm:h-screen sm:align-middle" aria-hidden="true">&#8203;</span>
            <div
              class="inline-block transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left align-bottom shadow-xl transition-all dark:bg-gray-800 sm:my-8 sm:w-full sm:max-w-lg sm:p-6 sm:align-middle"
            >
              <div class="absolute top-0 right-0 pt-4 pr-4">
                <button
                  type="button"
                  class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:bg-gray-800 dark:text-gray-500 dark:hover:text-gray-400"
                  @click="showVolunteerModal = false"
                >
                  <XIcon class="h-6 w-6" />
                </button>
              </div>
              <div>
                <h3 class="text-lg font-medium leading-6 text-gray-900 dark:text-white">Add New Volunteer</h3>
                <div class="mt-6 space-y-4">
                  <div class="grid grid-cols-2 gap-4">
                    <div>
                      <label for="first-name" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                        First Name
                      </label>
                      <input
                        type="text"
                        id="first-name"
                        class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                      />
                    </div>
                    <div>
                      <label for="last-name" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                        Last Name
                      </label>
                      <input
                        type="text"
                        id="last-name"
                        class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                      />
                    </div>
                  </div>
                  <div>
                    <label for="email" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Email
                    </label>
                    <input
                      type="email"
                      id="email"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div>
                    <label for="phone" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Phone
                    </label>
                    <input
                      type="tel"
                      id="phone"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div>
                    <label for="department" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Department
                    </label>
                    <select
                      id="department"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                    >
                      <option>Select a department</option>
                      <option>Worship</option>
                      <option>Children's Ministry</option>
                      <option>Hospitality</option>
                      <option>Technical</option>
                      <option>Outreach</option>
                    </select>
                  </div>
                  <div>
                    <label for="roles" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Roles
                    </label>
                    <div class="mt-2 space-y-2">
                      <div class="flex items-center">
                        <input
                          id="role-usher"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="role-usher" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Usher
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="role-greeter"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="role-greeter" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Greeter
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="role-worship"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="role-worship" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Worship Team
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="role-tech"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="role-tech" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Tech Team
                        </label>
                      </div>
                    </div>
                  </div>
                  <div>
                    <label for="notes" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Notes
                    </label>
                    <textarea
                      id="notes"
                      rows="3"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    ></textarea>
                  </div>
                </div>
                <div class="mt-6 flex justify-end space-x-3">
                  <button
                    type="button"
                    class="rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600"
                    @click="showVolunteerModal = false"
                  >
                    Cancel
                  </button>
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
                    @click="addVolunteer"
                  >
                    Add Volunteer
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </Teleport>
  
      <!-- Create Role Modal -->
      <Teleport to="body">
        <div v-if="showRoleModal" class="fixed inset-0 z-50 overflow-y-auto">
          <div class="flex min-h-screen items-end justify-center px-4 pt-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="showRoleModal = false"></div>
            <span class="hidden sm:inline-block sm:h-screen sm:align-middle" aria-hidden="true">&#8203;</span>
            <div
              class="inline-block transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left align-bottom shadow-xl transition-all dark:bg-gray-800 sm:my-8 sm:w-full sm:max-w-lg sm:p-6 sm:align-middle"
            >
              <div class="absolute top-0 right-0 pt-4 pr-4">
                <button
                  type="button"
                  class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:bg-gray-800 dark:text-gray-500 dark:hover:text-gray-400"
                  @click="showRoleModal = false"
                >
                  <XIcon class="h-6 w-6" />
                </button>
              </div>
              <div>
                <h3 class="text-lg font-medium leading-6 text-gray-900 dark:text-white">Create New Role</h3>
                <div class="mt-6 space-y-4">
                  <div>
                    <label for="role-name" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Role Name
                    </label>
                    <input
                      type="text"
                      id="role-name"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div>
                    <label for="role-department" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Department
                    </label>
                    <select
                      id="role-department"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                    >
                      <option>Select a department</option>
                      <option>Worship</option>
                      <option>Children's Ministry</option>
                      <option>Hospitality</option>
                      <option>Technical</option>
                      <option>Outreach</option>
                      <option>Create New Department</option>
                    </select>
                  </div>
                  <div>
                    <label for="role-description" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Description
                    </label>
                    <textarea
                      id="role-description"
                      rows="3"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    ></textarea>
                  </div>
                  <div>
                    <label for="role-volunteers-needed" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Volunteers Needed
                    </label>
                    <input
                      type="number"
                      id="role-volunteers-needed"
                      min="1"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div>
                    <label for="role-requirements" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Requirements
                    </label>
                    <div class="mt-2 space-y-2">
                      <div class="flex items-center">
                        <input
                          id="req-training"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="req-training" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Training Required
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="req-background"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="req-background" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Background Check Required
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="req-interview"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="req-interview" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Interview Required
                        </label>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="mt-6 flex justify-end space-x-3">
                  <button
                    type="button"
                    class="rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600"
                    @click="showRoleModal = false"
                  >
                    Cancel
                  </button>
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
                    @click="createRole"
                  >
                    Create Role
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </Teleport>
  
      <!-- Assign Volunteers Modal -->
      <Teleport to="body">
        <div v-if="assignmentData" class="fixed inset-0 z-50 overflow-y-auto">
          <div class="flex min-h-screen items-end justify-center px-4 pt-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="assignmentData = null"></div>
            <span class="hidden sm:inline-block sm:h-screen sm:align-middle" aria-hidden="true">&#8203;</span>
            <div
              class="inline-block transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left align-bottom shadow-xl transition-all dark:bg-gray-800 sm:my-8 sm:w-full sm:max-w-lg sm:p-6 sm:align-middle"
            >
              <div class="absolute top-0 right-0 pt-4 pr-4">
                <button
                  type="button"
                  class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:bg-gray-800 dark:text-gray-500 dark:hover:text-gray-400"
                  @click="assignmentData = null"
                >
                  <XIcon class="h-6 w-6" />
                </button>
              </div>
              <div>
                <h3 class="text-lg font-medium leading-6 text-gray-900 dark:text-white">
                  Assign Volunteers to {{ assignmentData.role.name }}
                </h3>
                <p class="mt-1 text-sm text-gray-500 dark:text-gray-400">
                  Department: {{ assignmentData.department.name }}
                </p>
                <div class="mt-6 space-y-4">
                  <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Available Volunteers
                    </label>
                    <div class="mt-2 max-h-60 overflow-y-auto rounded-md border border-gray-300 dark:border-gray-600">
                      <div
                        v-for="(volunteer, index) in availableVolunteers"
                        :key="index"
                        class="flex items-center justify-between border-b border-gray-200 px-4 py-2 last:border-b-0 dark:border-gray-700"
                      >
                        <div class="flex items-center">
                          <input
                            :id="`volunteer-${index}`"
                            type="checkbox"
                            class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                          />
                          <label :for="`volunteer-${index}`" class="ml-3 block text-sm font-medium text-gray-700 dark:text-gray-300">
                            <div class="flex items-center">
                              <img class="h-8 w-8 rounded-full" :src="volunteer.avatar" alt="" />
                              <span class="ml-2">{{ volunteer.name }}</span>
                            </div>
                          </label>
                        </div>
                        <div class="text-xs text-gray-500 dark:text-gray-400">
                          {{ volunteer.roles.join(', ') }}
                        </div>
                      </div>
                    </div>
                  </div>
                  <div>
                    <label for="assignment-date" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Assignment Date
                    </label>
                    <input
                      type="date"
                      id="assignment-date"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                    />
                  </div>
                  <div>
                    <label for="assignment-notes" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Notes
                    </label>
                    <textarea
                      id="assignment-notes"
                      rows="3"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    ></textarea>
                  </div>
                </div>
                <div class="mt-6 flex justify-end space-x-3">
                  <button
                    type="button"
                    class="rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600"
                    @click="assignmentData = null"
                  >
                    Cancel
                  </button>
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
                    @click="assignVolunteers"
                  >
                    Assign Volunteers
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </Teleport>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  import {
    Plus as PlusIcon,
    UserPlus as UserPlusIcon,
    Users as UsersIcon,
    Search as SearchIcon,
    X as XIcon,
    Pencil as PencilIcon,
    // DotsVertical as DotsVerticalIcon
  } from 'lucide-vue-next';

  definePageMeta({
    layout: 'dashboard'
  })
  
  // Departments and roles data
  const departments = [
    {
      name: 'Worship',
      description: 'Music, singing, and creative arts',
      roles: [
        { name: 'Worship Leader', assigned: 2, needed: 3 },
        { name: 'Vocalist', assigned: 5, needed: 6 },
        { name: 'Instrumentalist', assigned: 4, needed: 8 },
        { name: 'Audio Engineer', assigned: 1, needed: 2 }
      ]
    },
    {
      name: 'Children\'s Ministry',
      description: 'Nursery, Sunday School, and youth programs',
      roles: [
        { name: 'Teacher', assigned: 6, needed: 8 },
        { name: 'Assistant', assigned: 4, needed: 6 },
        { name: 'Nursery Worker', assigned: 3, needed: 4 }
      ]
    },
    {
      name: 'Hospitality',
      description: 'Greeting, ushering, and refreshments',
      roles: [
        { name: 'Greeter', assigned: 4, needed: 6 },
        { name: 'Usher', assigned: 6, needed: 8 },
        { name: 'Coffee Service', assigned: 2, needed: 4 }
      ]
    },
    {
      name: 'Technical',
      description: 'Sound, video, lighting, and livestream',
      roles: [
        { name: 'Sound Technician', assigned: 2, needed: 3 },
        { name: 'Video Operator', assigned: 1, needed: 2 },
        { name: 'Livestream Technician', assigned: 1, needed: 2 }
      ]
    },
    {
      name: 'Outreach',
      description: 'Community service and evangelism',
      roles: [
        { name: 'Team Leader', assigned: 2, needed: 3 },
        { name: 'Volunteer', assigned: 8, needed: 15 }
      ]
    }
  ];
  
  // Volunteers data
  const volunteers = [
    {
      name: 'Sarah Johnson',
      email: 'sarah.j@example.com',
      phone: '(555) 123-4567',
      roles: ['Worship Leader', 'Vocalist'],
      status: 'Active',
      lastServed: 'Mar 1, 2025',
      memberSince: 'Jan 2020',
      avatar: 'https://randomuser.me/api/portraits/women/1.jpg'
    },
    {
      name: 'Michael Chen',
      email: 'michael.c@example.com',
      phone: '(555) 234-5678',
      roles: ['Usher', 'Coffee Service'],
      status: 'Active',
      lastServed: 'Feb 28, 2025',
      memberSince: 'Mar 2018',
      avatar: 'https://randomuser.me/api/portraits/men/2.jpg'
    },
    {
      name: 'Jessica Williams',
      email: 'jessica.w@example.com',
      phone: '(555) 345-6789',
      roles: ['Teacher', 'Nursery Worker'],
      status: 'On Leave',
      lastServed: 'Feb 15, 2025',
      memberSince: 'Sep 2019',
      avatar: 'https://randomuser.me/api/portraits/women/3.jpg'
    },
    {
      name: 'David Rodriguez',
      email: 'david.r@example.com',
      phone: '(555) 456-7890',
      roles: ['Sound Technician', 'Video Operator'],
      status: 'Active',
      lastServed: 'Mar 1, 2025',
      memberSince: 'Jun 2021',
      avatar: 'https://randomuser.me/api/portraits/men/4.jpg'
    },
    {
      name: 'Emily Thompson',
      email: 'emily.t@example.com',
      phone: '(555) 567-8901',
      roles: ['Greeter'],
      status: 'Active',
      lastServed: 'Feb 22, 2025',
      memberSince: 'Nov 2022',
      avatar: 'https://randomuser.me/api/portraits/women/5.jpg'
    },
    {
      name: 'James Wilson',
      email: 'james.w@example.com',
      phone: '(555) 678-9012',
      roles: ['Instrumentalist'],
      status: 'Active',
      lastServed: 'Mar 1, 2025',
      memberSince: 'Apr 2020',
      avatar: 'https://randomuser.me/api/portraits/men/6.jpg'
    },
    {
      name: 'Olivia Martinez',
      email: 'olivia.m@example.com',
      phone: '(555) 789-0123',
      roles: ['Team Leader', 'Volunteer'],
      status: 'Active',
      lastServed: 'Feb 25, 2025',
      memberSince: 'Jan 2019',
      avatar: 'https://randomuser.me/api/portraits/women/7.jpg'
    },
    {
      name: 'Daniel Lee',
      email: 'daniel.l@example.com',
      phone: '(555) 890-1234',
      roles: ['Assistant'],
      status: 'Inactive',
      lastServed: 'Jan 12, 2025',
      memberSince: 'Jul 2022',
      avatar: 'https://randomuser.me/api/portraits/men/8.jpg'
    },
    {
      name: 'Sophia Garcia',
      email: 'sophia.g@example.com',
      phone: '(555) 901-2345',
      roles: ['Vocalist', 'Worship Leader'],
      status: 'Active',
      lastServed: 'Feb 28, 2025',
      memberSince: 'Dec 2020',
      avatar: 'https://randomuser.me/api/portraits/women/9.jpg'
    },
    {
      name: 'Ethan Brown',
      email: 'ethan.b@example.com',
      phone: '(555) 012-3456',
      roles: ['Livestream Technician'],
      status: 'Active',
      lastServed: 'Mar 1, 2025',
      memberSince: 'Feb 2021',
      avatar: 'https://randomuser.me/api/portraits/men/10.jpg'
    }
  ];
  
  // Available volunteers for assignment
  const availableVolunteers = [
    {
      name: 'Sarah Johnson',
      roles: ['Worship Leader', 'Vocalist'],
      avatar: 'https://randomuser.me/api/portraits/women/1.jpg'
    },
    {
      name: 'Michael Chen',
      roles: ['Usher', 'Coffee Service'],
      avatar: 'https://randomuser.me/api/portraits/men/2.jpg'
    },
    {
      name: 'Jessica Williams',
      roles: ['Teacher', 'Nursery Worker'],
      avatar: 'https://randomuser.me/api/portraits/women/3.jpg'
    },
    {
      name: 'David Rodriguez',
      roles: ['Sound Technician', 'Video Operator'],
      avatar: 'https://randomuser.me/api/portraits/men/4.jpg'
    },
    {
      name: 'Emily Thompson',
      roles: ['Greeter'],
      avatar: 'https://randomuser.me/api/portraits/women/5.jpg'
    }
  ];
  
  // Modal states
  const showVolunteerModal = ref(false);
  const showRoleModal = ref(false);
  const assignmentData = ref(null);
  
  // Get role badge class based on role name
  const getRoleBadgeClass = (role) => {
    const roleMap = {
      'Worship Leader': 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300',
      'Vocalist': 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300',
      'Instrumentalist': 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300',
      'Teacher': 'bg-blue-100 text-blue-800 dark:bg-blue-900/30 dark:text-blue-300',
      'Assistant': 'bg-blue-100 text-blue-800 dark:bg-blue-900/30 dark:text-blue-300',
      'Nursery Worker': 'bg-blue-100 text-blue-800 dark:bg-blue-900/30 dark:text-blue-300',
      'Greeter': 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300',
      'Usher': 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300',
      'Coffee Service': 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300',
      'Sound Technician': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300',
      'Video Operator': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300',
      'Livestream Technician': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300',
      'Team Leader': 'bg-red-100 text-red-800 dark:bg-red-900/30 dark:text-red-300',
      'Volunteer': 'bg-red-100 text-red-800 dark:bg-red-900/30 dark:text-red-300',
      'Audio Engineer': 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300'
    };
  
    return roleMap[role] || 'bg-gray-100 text-gray-800 dark:bg-gray-700 dark:text-gray-300';
  };
  
  // Open assign volunteers modal
  const openAssignVolunteers = (department, role) => {
    assignmentData.value = { department, role };
  };
  
  // Add volunteer
  const addVolunteer = () => {
    // Logic to add a new volunteer would go here
    showVolunteerModal.value = false;
    // Show success notification
    alert('Volunteer added successfully!');
  };
  
  // Create role
  const createRole = () => {
    // Logic to create a new role would go here
    showRoleModal.value = false;
    // Show success notification
    alert('Role created successfully!');
  };
  
  // Assign volunteers
  const assignVolunteers = () => {
    // Logic to assign volunteers would go here
    assignmentData.value = null;
    // Show success notification
    alert('Volunteers assigned successfully!');
  };
  </script>