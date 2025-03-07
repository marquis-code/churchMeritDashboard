<template>
  <div
    :class="[
      'fixed inset-y-0 z-50 flex w-64 flex-col transition-all duration-300 lg:left-0',
      isOpen ? 'left-0' : '-left-64'
    ]"
  >
    <!-- Sidebar backdrop for mobile -->
    <div
      v-if="isOpen"
      class="fixed inset-0 bg-gray-900/80 lg:hidden"
      @click="isOpen = false"
    ></div>

    <!-- Sidebar content -->
    <div class="flex h-full flex-col overflow-y-auto bg-white dark:bg-gray-800 shadow-lg">
      <!-- Logo -->
      <div class="flex h-16 items-center border-b border-gray-200 dark:border-gray-700 px-6">
        <div class="flex items-center gap-2">
          <div class="rounded-full bg-purple-600 p-1.5">
            <Church class="h-5 w-5 text-white" />
          </div>
          <span class="text-xl font-semibold text-gray-900 dark:text-white">ChurchManager</span>
        </div>
      </div>

      <!-- Navigation -->
      <nav class="flex-1 space-y-1 px-4 py-4">
        <div v-for="(section, i) in navigation" :key="i" class="py-2">
          <!-- Section header with toggle button -->
          <button 
            @click="toggleSection(i)"
            class="flex w-full items-center justify-between px-3 py-2 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase tracking-wider hover:text-gray-700 dark:hover:text-gray-300"
          >
            <span>{{ section.name }}</span>
            <ChevronDown 
              :class="[
                'h-4 w-4 transition-transform duration-200',
                expandedSectionIndex === i ? 'rotate-180' : ''
              ]" 
            />
          </button>
          
          <!-- Dropdown content with animation -->
          <transition
            enter-active-class="transition duration-200 ease-out"
            enter-from-class="transform -translate-y-2 opacity-0"
            enter-to-class="transform translate-y-0 opacity-100"
            leave-active-class="transition duration-150 ease-in"
            leave-from-class="transform translate-y-0 opacity-100"
            leave-to-class="transform -translate-y-2 opacity-0"
          >
            <div v-show="expandedSectionIndex === i" class="mt-1 space-y-1 overflow-hidden">
              <NuxtLink
                v-for="item in section.items"
                :key="item.name"
                :to="item.href"
                class="group flex items-center px-3 py-2 text-sm font-medium rounded-md ml-2"
                :class="[
                  isActiveRoute(item.href)
                    ? 'bg-purple-100 text-purple-700 dark:bg-purple-900/50 dark:text-purple-300'
                    : 'text-gray-700 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-700'
                ]"
              >
                <component :is="item.icon" class="mr-3 h-5 w-5 flex-shrink-0" :class="[
                  isActiveRoute(item.href)
                    ? 'text-purple-600 dark:text-purple-400'
                    : 'text-gray-500 dark:text-gray-400'
                ]" />
                {{ item.name }}
              </NuxtLink>
            </div>
          </transition>
        </div>
      </nav>

      <!-- User profile -->
      <div class="border-t border-gray-200 dark:border-gray-700 p-4">
        <div class="flex items-center gap-3">
          <img
            src="https://randomuser.me/api/portraits/men/42.jpg"
            alt="User avatar"
            class="h-10 w-10 rounded-full"
          />
          <div>
            <p class="text-sm font-medium text-gray-900 dark:text-white">Pastor John Doe</p>
            <p class="text-xs text-gray-500 dark:text-gray-400">Admin</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Mobile menu button -->
  <button
    type="button"
    class="fixed bottom-4 right-4 z-50 rounded-full bg-purple-600 p-3 text-white shadow-lg lg:hidden"
    @click="isOpen = !isOpen"
  >
    <Menu v-if="!isOpen" class="h-6 w-6" />
    <X v-else class="h-6 w-6" />
  </button>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import {
  Calendar,
  Video,
  Users,
  UserCheck,
  BookOpen,
  Bell,
  Building,
  Package,
  FileText,
  AlertTriangle,
  DollarSign,
  Receipt,
  Menu,
  X,
  Church,
  ChevronDown
} from 'lucide-vue-next';

const route = useRoute();
const isOpen = ref(false);

// Track which section is expanded (-1 means none)
const expandedSectionIndex = ref<number>(-1);

// Toggle section expansion
const toggleSection = (index: number) => {
  // If clicking on the already expanded section, close it
  if (expandedSectionIndex.value === index) {
    expandedSectionIndex.value = -1;
  } else {
    // Otherwise, open the clicked section (and close any other)
    expandedSectionIndex.value = index;
  }
};

// Check if a route is active
const isActiveRoute = (href: string): boolean => {
  return route.path.includes(href);
};

// Find and expand the section containing the active route
const expandActiveSection = () => {
  for (let i = 0; i < navigation.length; i++) {
    if (navigation[i].items.some(item => isActiveRoute(item.href))) {
      expandedSectionIndex.value = i;
      break;
    }
  }
};

// Navigation data
const navigation = [
  {
    name: 'Event Management',
    items: [
      { name: 'Event Scheduling', href: '/events/scheduling', icon: Calendar },
      { name: 'Live Streaming & Media', href: '/events/media', icon: Video },
    ],
  },
  {
    name: 'Volunteer & Workforce',
    items: [
      { name: 'Volunteer Assignments', href: '/volunteers/assignments', icon: Users },
      { name: 'Training & Certification', href: '/volunteers/training', icon: UserCheck },
    ],
  },
  {
    name: 'Facility & Asset',
    items: [
      { name: 'Facility Booking', href: '/facilities/booking', icon: Building },
      { name: 'Asset Tracking', href: '/facilities/assets', icon: Package },
    ],
  },
  {
    name: 'Member Engagement',
    items: [
      { name: 'Small Groups', href: '/members/groups', icon: BookOpen },
      { name: 'Automated Follow-ups', href: '/members/followups', icon: Bell },
    ],
  },
  {
    name: 'Legal & Compliance',
    items: [
      { name: 'Document Storage', href: '/legal/documents', icon: FileText },
      { name: 'Regulatory Compliance', href: '/legal/compliance', icon: AlertTriangle },
    ],
  },
  {
    name: 'Payments & Donations',
    items: [
      { name: 'Payment Collection', href: '/payments/collection', icon: DollarSign },
      { name: 'Payment History', href: '/payments/history', icon: Receipt },
    ],
  },
];

// Initialize expanded section on component creation
onMounted(() => {
  expandActiveSection();
});
</script>