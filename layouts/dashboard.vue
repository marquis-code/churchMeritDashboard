<template>
    <div class="min-h-screen bg-gray-50 dark:bg-gray-900">
      <AppSidebar />
      <div class="lg:pl-64">
        <AppHeader />
        <main class="p-4 sm:p-6 lg:p-8">
          <slot />
        </main>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  // Main app layout
  </script>