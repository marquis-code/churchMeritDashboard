<template>
    <div>
      <div class="mb-6 flex items-center justify-between">
        <h2 class="text-2xl font-bold text-gray-900 dark:text-white">Event Scheduling</h2>
        <button
          @click="showEventModal = true"
          class="inline-flex items-center rounded-md bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
        >
          <PlusIcon class="mr-2 h-4 w-4" />
          New Event
        </button>
      </div>
  
      <!-- Calendar view -->
      <div class="mb-8 rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800">
        <div class="mb-4 flex items-center justify-between">
          <div class="flex items-center space-x-2">
            <button class="rounded-md p-1 hover:bg-gray-100 dark:hover:bg-gray-700">
              <ChevronLeftIcon class="h-5 w-5 text-gray-500 dark:text-gray-400" />
            </button>
            <h3 class="text-lg font-medium text-gray-900 dark:text-white">March 2025</h3>
            <button class="rounded-md p-1 hover:bg-gray-100 dark:hover:bg-gray-700">
              <ChevronRightIcon class="h-5 w-5 text-gray-500 dark:text-gray-400" />
            </button>
          </div>
          <div class="flex space-x-2">
            <button
              class="rounded-md border border-gray-300 px-3 py-1 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-600 dark:text-gray-300 dark:hover:bg-gray-700"
            >
              Month
            </button>
            <button
              class="rounded-md border border-gray-300 px-3 py-1 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-600 dark:text-gray-300 dark:hover:bg-gray-700"
            >
              Week
            </button>
            <button
              class="rounded-md border border-gray-300 px-3 py-1 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-600 dark:text-gray-300 dark:hover:bg-gray-700"
            >
              Day
            </button>
          </div>
        </div>
  
        <!-- Calendar grid -->
        <div class="grid grid-cols-7 gap-px bg-gray-200 dark:bg-gray-700">
          <!-- Day headers -->
          <div
            v-for="day in ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']"
            :key="day"
            class="bg-gray-100 p-2 text-center text-sm font-medium text-gray-500 dark:bg-gray-800 dark:text-gray-400"
          >
            {{ day }}
          </div>
  
          <!-- Calendar days -->
          <div
            v-for="(day, index) in calendarDays"
            :key="index"
            class="min-h-[100px] bg-white p-2 dark:bg-gray-800"
            :class="{
              'opacity-50': day.isCurrentMonth === false,
              'bg-purple-50 dark:bg-purple-900/20': day.isToday
            }"
          >
            <div class="flex justify-between">
              <span
                class="text-sm font-medium"
                :class="[
                  day.isToday
                    ? 'rounded-full bg-purple-600 h-6 w-6 flex items-center justify-center text-white'
                    : 'text-gray-900 dark:text-white'
                ]"
              >
                {{ day.date }}
              </span>
            </div>
            <!-- Events for this day -->
            <div class="mt-1 space-y-1">
              <div
                v-for="(event, eventIndex) in day.events"
                :key="eventIndex"
                class="cursor-pointer rounded px-1 py-0.5 text-xs font-medium"
                :class="event.colorClass"
                @click="openEventDetails(event)"
              >
                {{ event.title }}
              </div>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Upcoming events list -->
      <div class="rounded-lg border border-gray-200 bg-white shadow-sm dark:border-gray-700 dark:bg-gray-800">
        <div class="border-b border-gray-200 px-6 py-4 dark:border-gray-700">
          <h3 class="text-lg font-medium text-gray-900 dark:text-white">Upcoming Events</h3>
        </div>
        <ul class="divide-y divide-gray-200 dark:divide-gray-700">
          <li v-for="(event, index) in upcomingEvents" :key="index" class="px-6 py-4">
            <div class="flex items-center justify-between">
              <div class="flex items-center">
                <div class="flex h-10 w-10 items-center justify-center rounded-full" :class="event.colorBg">
                  <component :is="event.icon" class="h-5 w-5 text-white" />
                </div>
                <div class="ml-4">
                  <h4 class="text-sm font-medium text-gray-900 dark:text-white">{{ event.title }}</h4>
                  <div class="mt-1 flex items-center text-xs text-gray-500 dark:text-gray-400">
                    <CalendarIcon class="mr-1 h-3 w-3" />
                    {{ event.date }}
                    <ClockIcon class="ml-3 mr-1 h-3 w-3" />
                    {{ event.time }}
                    <MapPinIcon class="ml-3 mr-1 h-3 w-3" />
                    {{ event.location }}
                  </div>
                </div>
              </div>
              <div class="flex items-center space-x-2">
                <button
                  class="rounded-md bg-white px-2.5 py-1.5 text-sm font-medium text-gray-700 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 dark:bg-gray-700 dark:text-gray-300 dark:ring-gray-600 dark:hover:bg-gray-600"
                  @click="openEventDetails(event)"
                >
                  Details
                </button>
                <button
                  class="rounded-md bg-purple-50 px-2.5 py-1.5 text-sm font-medium text-purple-700 shadow-sm ring-1 ring-inset ring-purple-700/10 hover:bg-purple-100 dark:bg-purple-900/20 dark:text-purple-300 dark:ring-purple-700/30 dark:hover:bg-purple-900/30"
                >
                  Assign
                </button>
              </div>
            </div>
          </li>
        </ul>
      </div>
  
      <!-- New Event Modal -->
      <Teleport to="body">
        <div v-if="showEventModal" class="fixed inset-0 z-50 overflow-y-auto">
          <div class="flex min-h-screen items-end justify-center px-4 pt-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="showEventModal = false"></div>
            <span class="hidden sm:inline-block sm:h-screen sm:align-middle" aria-hidden="true">&#8203;</span>
            <div
              class="inline-block transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left align-bottom shadow-xl transition-all dark:bg-gray-800 sm:my-8 sm:w-full sm:max-w-lg sm:p-6 sm:align-middle"
            >
              <div class="absolute top-0 right-0 pt-4 pr-4">
                <button
                  type="button"
                  class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:bg-gray-800 dark:text-gray-500 dark:hover:text-gray-400"
                  @click="showEventModal = false"
                >
                  <XIcon class="h-6 w-6" />
                </button>
              </div>
              <div>
                <h3 class="text-lg font-medium leading-6 text-gray-900 dark:text-white">Create New Event</h3>
                <div class="mt-6 space-y-4">
                  <div>
                    <label for="event-title" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Event Title
                    </label>
                    <input
                      type="text"
                      id="event-title"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div class="grid grid-cols-2 gap-4">
                    <div>
                      <label for="event-date" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                        Date
                      </label>
                      <input
                        type="date"
                        id="event-date"
                        class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                      />
                    </div>
                    <div>
                      <label for="event-time" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                        Time
                      </label>
                      <input
                        type="time"
                        id="event-time"
                        class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                      />
                    </div>
                  </div>
                  <div>
                    <label for="event-location" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Location
                    </label>
                    <input
                      type="text"
                      id="event-location"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    />
                  </div>
                  <div>
                    <label for="event-type" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Event Type
                    </label>
                    <select
                      id="event-type"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white"
                    >
                      <option>Church Service</option>
                      <option>Special Event</option>
                      <option>Meeting</option>
                      <option>Bible Study</option>
                      <option>Community Outreach</option>
                    </select>
                  </div>
                  <div>
                    <label for="event-description" class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Description
                    </label>
                    <textarea
                      id="event-description"
                      rows="3"
                      class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-purple-500 focus:ring-purple-500 dark:border-gray-600 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400"
                    ></textarea>
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-700 dark:text-gray-300">
                      Assign Resources
                    </label>
                    <div class="mt-2 space-y-2">
                      <div class="flex items-center">
                        <input
                          id="assign-speakers"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="assign-speakers" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Speakers
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="assign-choirs"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="assign-choirs" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Choirs
                        </label>
                      </div>
                      <div class="flex items-center">
                        <input
                          id="assign-volunteers"
                          type="checkbox"
                          class="h-4 w-4 rounded border-gray-300 text-purple-600 focus:ring-purple-500 dark:border-gray-600"
                        />
                        <label for="assign-volunteers" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
                          Volunteers
                        </label>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="mt-6 flex justify-end space-x-3">
                  <button
                    type="button"
                    class="rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600"
                    @click="showEventModal = false"
                  >
                    Cancel
                  </button>
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
                    @click="createEvent"
                  >
                    Create Event
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </Teleport>
  
      <!-- Event Details Modal -->
      <Teleport to="body">
        <div v-if="selectedEvent" class="fixed inset-0 z-50 overflow-y-auto">
          <div class="flex min-h-screen items-end justify-center px-4 pt-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="selectedEvent = null"></div>
            <span class="hidden sm:inline-block sm:h-screen sm:align-middle" aria-hidden="true">&#8203;</span>
            <div
              class="inline-block transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left align-bottom shadow-xl transition-all dark:bg-gray-800 sm:my-8 sm:w-full sm:max-w-lg sm:p-6 sm:align-middle"
            >
              <div class="absolute top-0 right-0 pt-4 pr-4">
                <button
                  type="button"
                  class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:bg-gray-800 dark:text-gray-500 dark:hover:text-gray-400"
                  @click="selectedEvent = null"
                >
                  <XIcon class="h-6 w-6" />
                </button>
              </div>
              <div>
                <div class="flex items-center">
                  <div class="flex h-12 w-12 items-center justify-center rounded-full" :class="selectedEvent.colorBg">
                    <component :is="selectedEvent.icon" class="h-6 w-6 text-white" />
                  </div>
                  <h3 class="ml-4 text-lg font-medium leading-6 text-gray-900 dark:text-white">
                    {{ selectedEvent.title }}
                  </h3>
                </div>
                <div class="mt-4 space-y-3">
                  <div class="flex items-center text-sm text-gray-500 dark:text-gray-400">
                    <CalendarIcon class="mr-2 h-5 w-5" />
                    {{ selectedEvent.date }}
                  </div>
                  <div class="flex items-center text-sm text-gray-500 dark:text-gray-400">
                    <ClockIcon class="mr-2 h-5 w-5" />
                    {{ selectedEvent.time }}
                  </div>
                  <div class="flex items-center text-sm text-gray-500 dark:text-gray-400">
                    <MapPinIcon class="mr-2 h-5 w-5" />
                    {{ selectedEvent.location }}
                  </div>
                  <div class="pt-2">
                    <h4 class="text-sm font-medium text-gray-700 dark:text-gray-300">Description</h4>
                    <p class="mt-1 text-sm text-gray-500 dark:text-gray-400">
                      {{ selectedEvent.description || 'No description provided.' }}
                    </p>
                  </div>
                  <div class="pt-2">
                    <h4 class="text-sm font-medium text-gray-700 dark:text-gray-300">Assigned Resources</h4>
                    <div class="mt-2 space-y-1">
                      <div v-if="selectedEvent.speakers && selectedEvent.speakers.length" class="flex items-start">
                        <MicIcon class="mr-2 h-5 w-5 text-gray-400" />
                        <div>
                          <p class="text-xs font-medium text-gray-700 dark:text-gray-300">Speakers</p>
                          <p class="text-sm text-gray-500 dark:text-gray-400">
                            {{ selectedEvent.speakers.join(', ') }}
                          </p>
                        </div>
                      </div>
                      <div v-if="selectedEvent.volunteers && selectedEvent.volunteers.length" class="flex items-start">
                        <UsersIcon class="mr-2 h-5 w-5 text-gray-400" />
                        <div>
                          <p class="text-xs font-medium text-gray-700 dark:text-gray-300">Volunteers</p>
                          <p class="text-sm text-gray-500 dark:text-gray-400">
                            {{ selectedEvent.volunteers.join(', ') }}
                          </p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="mt-6 flex justify-end space-x-3">
                  <button
                    type="button"
                    class="rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600"
                    @click="selectedEvent = null"
                  >
                    Close
                  </button>
                  <button
                    type="button"
                    class="inline-flex justify-center rounded-md border border-transparent bg-purple-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2"
                  >
                    Edit Event
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
    ChevronLeft as ChevronLeftIcon,
    ChevronRight as ChevronRightIcon,
    Calendar as CalendarIcon,
    Clock as ClockIcon,
    MapPin as MapPinIcon,
    X as XIcon,
    Music as MusicIcon,
    Video as VideoIcon,
    BookOpen as BookOpenIcon,
    Heart as HeartIcon,
    Users as UsersIcon,
    Mic as MicIcon
  } from 'lucide-vue-next';

  definePageMeta({
    layout: 'dashboard'
  })
  
  // Calendar data
  const calendarDays = [
    // Previous month
    { date: 25, isCurrentMonth: false, isToday: false, events: [] },
    { date: 26, isCurrentMonth: false, isToday: false, events: [] },
    { date: 27, isCurrentMonth: false, isToday: false, events: [] },
    { date: 28, isCurrentMonth: false, isToday: false, events: [] },
    { date: 29, isCurrentMonth: false, isToday: false, events: [] },
    // Current month
    { date: 1, isCurrentMonth: true, isToday: false, events: [] },
    { date: 2, isCurrentMonth: true, isToday: false, events: [] },
    { date: 3, isCurrentMonth: true, isToday: false, events: [
      { title: 'Prayer Meeting', colorClass: 'bg-blue-100 text-blue-800 dark:bg-blue-900/30 dark:text-blue-300' }
    ] },
    { date: 4, isCurrentMonth: true, isToday: false, events: [] },
    { date: 5, isCurrentMonth: true, isToday: false, events: [] },
    { date: 6, isCurrentMonth: true, isToday: true, events: [
      { title: 'Sunday Service', colorClass: 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300' }
    ] },
    { date: 7, isCurrentMonth: true, isToday: false, events: [] },
    { date: 8, isCurrentMonth: true, isToday: false, events: [] },
    { date: 9, isCurrentMonth: true, isToday: false, events: [
      { title: 'Youth Group', colorClass: 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300' }
    ] },
    { date: 10, isCurrentMonth: true, isToday: false, events: [] },
    { date: 11, isCurrentMonth: true, isToday: false, events: [] },
    { date: 12, isCurrentMonth: true, isToday: false, events: [] },
    { date: 13, isCurrentMonth: true, isToday: false, events: [
      { title: 'Sunday Service', colorClass: 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300' }
    ] },
    { date: 14, isCurrentMonth: true, isToday: false, events: [] },
    { date: 15, isCurrentMonth: true, isToday: false, events: [
      { title: 'Bible Study', colorClass: 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/30 dark:text-yellow-300' }
    ] },
    { date: 16, isCurrentMonth: true, isToday: false, events: [] },
    { date: 17, isCurrentMonth: true, isToday: false, events: [] },
    { date: 18, isCurrentMonth: true, isToday: false, events: [] },
    { date: 19, isCurrentMonth: true, isToday: false, events: [] },
    { date: 20, isCurrentMonth: true, isToday: false, events: [
      { title: 'Sunday Service', colorClass: 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300' },
      { title: 'Choir Practice', colorClass: 'bg-red-100 text-red-800 dark:bg-red-900/30 dark:text-red-300' }
    ] },
    { date: 21, isCurrentMonth: true, isToday: false, events: [] },
    { date: 22, isCurrentMonth: true, isToday: false, events: [] },
    { date: 23, isCurrentMonth: true, isToday: false, events: [
      { title: 'Youth Group', colorClass: 'bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300' }
    ] },
    { date: 24, isCurrentMonth: true, isToday: false, events: [] },
    { date: 25, isCurrentMonth: true, isToday: false, events: [] },
    { date: 26, isCurrentMonth: true, isToday: false, events: [] },
    { date: 27, isCurrentMonth: true, isToday: false, events: [
      { title: 'Sunday Service', colorClass: 'bg-purple-100 text-purple-800 dark:bg-purple-900/30 dark:text-purple-300' }
    ] },
    { date: 28, isCurrentMonth: true, isToday: false, events: [] },
    { date: 29, isCurrentMonth: true, isToday: false, events: [] },
    { date: 30, isCurrentMonth: true, isToday: false, events: [] },
    { date: 31, isCurrentMonth: true, isToday: false, events: [] },
    // Next month
    { date: 1, isCurrentMonth: false, isToday: false, events: [] },
    { date: 2, isCurrentMonth: false, isToday: false, events: [] },
    { date: 3, isCurrentMonth: false, isToday: false, events: [] },
    { date: 4, isCurrentMonth: false, isToday: false, events: [] },
    { date: 5, isCurrentMonth: false, isToday: false, events: [] },
  ];
  
  // Upcoming events data
  const upcomingEvents = [
    {
      title: 'Sunday Service',
      date: 'March 6, 2025',
      time: '10:00 AM - 12:00 PM',
      location: 'Main Sanctuary',
      icon: MusicIcon,
      colorBg: 'bg-purple-600',
      description: 'Regular Sunday worship service with praise and worship, announcements, and sermon.',
      speakers: ['Pastor John Doe'],
      volunteers: ['Worship Team', 'Ushers', 'Children\'s Ministry']
    },
    {
      title: 'Youth Bible Study',
      date: 'March 9, 2025',
      time: '6:30 PM - 8:00 PM',
      location: 'Youth Room',
      icon: BookOpenIcon,
      colorBg: 'bg-green-600',
      description: 'Weekly Bible study for teenagers and young adults.',
      speakers: ['Youth Pastor Sarah'],
      volunteers: ['Youth Leaders']
    },
    {
      title: 'Prayer Meeting',
      date: 'March 10, 2025',
      time: '7:00 PM - 8:30 PM',
      location: 'Prayer Room',
      icon: HeartIcon,
      colorBg: 'bg-blue-600',
      description: 'Mid-week prayer meeting for the congregation.',
      speakers: [],
      volunteers: ['Prayer Team']
    },
    {
      title: 'Community Outreach',
      date: 'March 13, 2025',
      time: '9:00 AM - 1:00 PM',
      location: 'Downtown Community Center',
      icon: UsersIcon,
      colorBg: 'bg-red-600',
      description: 'Monthly community service event providing meals and essentials to those in need.',
      speakers: [],
      volunteers: ['Outreach Team', 'Food Service Volunteers', 'Drivers']
    },
    {
      title: 'Choir Practice',
      date: 'March 20, 2025',
      time: '6:00 PM - 7:30 PM',
      location: 'Choir Room',
      icon: MusicIcon,
      colorBg: 'bg-yellow-600',
      description: 'Weekly practice for the church choir in preparation for Sunday service.',
      speakers: [],
      volunteers: ['Choir Director', 'Choir Members']
    }
  ];
  
  // Modal states
  const showEventModal = ref(false);
  const selectedEvent = ref(null);
  
  // Open event details
  const openEventDetails = (event) => {
    // Find the full event details from upcomingEvents
    const fullEvent = upcomingEvents.find(e => e.title === event.title) || event;
    selectedEvent.value = fullEvent;
  };
  
  // Create new event
  const createEvent = () => {
    // Logic to create a new event would go here
    showEventModal.value = false;
    // Show success notification
    alert('Event created successfully!');
  };
  </script>