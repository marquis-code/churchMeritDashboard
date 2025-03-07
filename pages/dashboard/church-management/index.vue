<template>
  <main>
    <div>
      <div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
        <!-- Dashboard cards -->
        <DashboardCard 
          title="Upcoming Events" 
          :value="12" 
          icon="Calendar" 
          color="bg-blue-500"
          to="/events/scheduling"
        />
        <DashboardCard 
          title="Active Volunteers" 
          :value="48" 
          icon="Users" 
          color="bg-green-500"
          to="/volunteers/assignments"
        />
        <DashboardCard 
          title="Small Groups" 
          :value="24" 
          icon="BookOpen" 
          color="bg-purple-500"
          to="/members/groups"
        />
        <DashboardCard 
          title="Facility Bookings" 
          :value="8" 
          icon="Building" 
          color="bg-orange-500"
          to="/facilities/booking"
        />
        <DashboardCard 
          title="This Month's Donations" 
          value="$12,450" 
          icon="DollarSign" 
          color="bg-emerald-500"
          to="/payments/collection"
        />
        <DashboardCard 
          title="Pending Approvals" 
          :value="5" 
          icon="AlertTriangle" 
          color="bg-red-500"
          to="/legal/compliance"
        />
      </div>
  
      <div class="mt-8 grid grid-cols-1 gap-6 lg:grid-cols-2">
        <!-- Recent events -->
        <div class="rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800">
          <div class="flex items-center justify-between mb-4">
            <h2 class="text-lg font-semibold text-gray-900 dark:text-white">Recent Events</h2>
            <NuxtLink to="/events/scheduling" class="text-sm font-medium text-purple-600 hover:text-purple-500 dark:text-purple-400">
              View all
            </NuxtLink>
          </div>
          <div class="space-y-4">
            <div v-for="(event, index) in recentEvents" :key="index" class="flex items-start gap-4">
              <div class="flex h-10 w-10 items-center justify-center rounded-full" :class="event.color">
                <component :is="event.icon" class="h-5 w-5 text-white" />
              </div>
              <div class="flex-1">
                <h3 class="font-medium text-gray-900 dark:text-white">{{ event.title }}</h3>
                <p class="text-sm text-gray-500 dark:text-gray-400">{{ event.date }}</p>
              </div>
            </div>
          </div>
        </div>
  
        <!-- Upcoming tasks -->
        <div class="rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800">
          <div class="flex items-center justify-between mb-4">
            <h2 class="text-lg font-semibold text-gray-900 dark:text-white">Upcoming Tasks</h2>
            <button class="text-sm font-medium text-purple-600 hover:text-purple-500 dark:text-purple-400">
              View all
            </button>
          </div>
          <div class="space-y-4">
            <div v-for="(task, index) in upcomingTasks" :key="index" class="flex items-center gap-3">
              <input type="checkbox" class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600" />
              <div class="flex-1">
                <p class="font-medium text-gray-900 dark:text-white">{{ task.title }}</p>
                <p class="text-sm text-gray-500 dark:text-gray-400">{{ task.dueDate }}</p>
              </div>
              <span class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium" :class="task.priorityClass">
                {{ task.priority }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
  </template>
  
  <script setup lang="ts">
  import { 
    Calendar, 
    Users, 
    BookOpen, 
    Building, 
    DollarSign, 
    AlertTriangle,
    Music,
    Video,
    Heart
  } from 'lucide-vue-next';
  
  
  // Recent events data
  const recentEvents = [
    { 
      title: 'Sunday Service', 
      date: 'Sunday, 10:00 AM', 
      icon: Music, 
      color: 'bg-blue-500' 
    },
    { 
      title: 'Youth Bible Study', 
      date: 'Wednesday, 6:30 PM', 
      icon: BookOpen, 
      color: 'bg-purple-500' 
    },
    { 
      title: 'Live Stream Setup', 
      date: 'Saturday, 2:00 PM', 
      icon: Video, 
      color: 'bg-green-500' 
    },
    { 
      title: 'Community Outreach', 
      date: 'Saturday, 9:00 AM', 
      icon: Heart, 
      color: 'bg-red-500' 
    },
  ];
  
  // Upcoming tasks data
  const upcomingTasks = [
    { 
      title: 'Prepare Sunday sermon slides', 
      dueDate: 'Due tomorrow', 
      priority: 'High', 
      priorityClass: 'bg-red-100 text-red-800 dark:bg-red-900/30 dark:text-red-300' 
    },
    { 
      title: 'Confirm volunteer schedule', 
      dueDate: 'Due in 2 days', 
      priority: 'Medium', 
      priorityClass: 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300' 
    },
    { 
      title: 'Review donation reports', 
      dueDate: 'Due in 3 days', 
      priority: 'Medium', 
      priorityClass: 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300' 
    },
    { 
      title: 'Update website event calendar', 
      dueDate: 'Due in 5 days', 
      priority: 'Low', 
      priorityClass: 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300' 
    },
  ];

  definePageMeta({
      layout: 'dashboard'
  })
  </script>