<template>
    <header class="sticky top-0 z-40 flex h-16 shrink-0 items-center gap-x-4 border-b border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 px-4 shadow-sm sm:gap-x-6 sm:px-6 lg:px-8">
      <div class="flex flex-1 items-center justify-between">
        <h1 class="text-xl font-semibold text-gray-900 dark:text-white">
          {{ pageTitle }}
        </h1>
        
        <div class="flex items-center gap-x-4 lg:gap-x-6">
          <!-- Search -->
          <div class="relative">
            <SearchIcon class="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-gray-400 dark:text-gray-500" />
            <input
              type="text"
              placeholder="Search..."
              class="h-10 w-full rounded-md border border-gray-300 bg-white pl-10 pr-4 text-sm text-gray-900 placeholder-gray-500 focus:border-purple-500 focus:outline-none focus:ring-1 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
            />
          </div>
  
          <!-- Notifications -->
          <button type="button" class="relative rounded-full p-1.5 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700">
            <BellIcon class="h-5 w-5" />
            <span class="absolute -top-0.5 -right-0.5 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[10px] font-bold text-white">3</span>
          </button>
  
          <!-- Theme toggle -->
          <button 
            type="button" 
            class="rounded-full p-1.5 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
            @click="toggleDarkMode"
          >
            <SunIcon v-if="isDarkMode" class="h-5 w-5" />
            <MoonIcon v-else class="h-5 w-5" />
          </button>
        </div>
      </div>
    </header>
  </template>
  
  <script setup lang="ts">
  import { computed, ref, onMounted } from 'vue';
  import { useRoute } from 'vue-router';
  import { 
    Search as SearchIcon, 
    Bell as BellIcon,
    Sun as SunIcon,
    Moon as MoonIcon
  } from 'lucide-vue-next';
  
  const route = useRoute();
  const isDarkMode = ref(false);
  
  // Page title based on current route
  const pageTitle = computed(() => {
    const path = route.path;
    
    if (path.includes('/events/scheduling')) return 'Event Scheduling';
    if (path.includes('/events/media')) return 'Live Streaming & Media Archive';
    if (path.includes('/volunteers/assignments')) return 'Volunteer Assignments';
    if (path.includes('/volunteers/training')) return 'Workforce Training & Certification';
    if (path.includes('/facilities/booking')) return 'Facility Booking';
    if (path.includes('/facilities/assets')) return 'Asset Tracking';
    if (path.includes('/members/groups')) return 'Small Group & Discipleship Management';
    if (path.includes('/members/followups')) return 'Automated Follow-ups';
    if (path.includes('/legal/documents')) return 'Document Storage';
    if (path.includes('/legal/compliance')) return 'Regulatory Compliance Tracking';
    if (path.includes('/payments/collection')) return 'Payment Collection';
    if (path.includes('/payments/history')) return 'Payment History & Receipts';
    
    return 'Dashboard';
  });
  
  // Toggle dark mode
  const toggleDarkMode = () => {
    isDarkMode.value = !isDarkMode.value;
    if (isDarkMode.value) {
      document.documentElement.classList.add('dark');
      localStorage.setItem('darkMode', 'true');
    } else {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('darkMode', 'false');
    }
  };
  
  // Check for saved dark mode preference
  onMounted(() => {
    const savedDarkMode = localStorage.getItem('darkMode');
    if (savedDarkMode === 'true' || 
        (!savedDarkMode && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
      isDarkMode.value = true;
      document.documentElement.classList.add('dark');
    }
  });
  </script>