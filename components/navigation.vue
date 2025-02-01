<template>
  <nav class="bg-white shadow-lg fixed w-full z-50">
    <div class="max-w-7xl mx-auto px-4">
      <div class="flex justify-between items-center h-16">
        <!-- Logo -->
        <NuxtLink to="/" class="flex-shrink-0">
          <img class="h-8 w-8" src="#" alt="Logo">
        </NuxtLink>

        <!-- Desktop Menu -->
        <div class="hidden md:flex space-x-8">
          <NuxtLink v-for="(item, index) in navigation" :key="index" :to="item.path"
            class="text-gray-700 hover:text-blue-500 px-3 py-2 rounded-md text-sm font-medium"
            :class="{ 'text-blue-500': route.path === item.path }">
            {{ item.name }}
          </NuxtLink>
        </div>

        <!-- Mobile Menu Button -->
        <div class="md:hidden">
          <button @click="toggleMenu"
            class="inline-flex items-center justify-center p-2 rounded-md text-gray-700 hover:text-blue-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-blue-500">
            <svg class="h-6 w-6" :class="{ 'hidden': showMenu, 'block': !showMenu }" xmlns="http://www.w3.org/2000/svg"
              fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
            <svg class="h-6 w-6" :class="{ 'hidden': !showMenu, 'block': showMenu }" xmlns="http://www.w3.org/2000/svg"
              fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Full-Screen Mobile Menu -->
    <Transition name="fullscreen-slide">
      <div v-if="showMenu"
        class="fixed top-0 left-0 w-full h-full bg-white z-40 flex flex-col items-center justify-center space-y-6">
        <button @click="toggleMenu" class="absolute top-5 right-5 text-gray-700 hover:text-blue-500 text-3xl">
          &times;
        </button>
        <NuxtLink v-for="(item, index) in navigation" :key="index" :to="item.path"
          class="text-gray-700 hover:text-blue-500 text-2xl font-semibold" @click="toggleMenu"
          :class="{ 'text-blue-500': route.path === item.path }">
          {{ item.name }}
        </NuxtLink>
      </div>
    </Transition>
  </nav>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const showMenu = ref(false);

// Navigation items
const navigation = ref([
  { name: 'Home', path: '/' },
  { name: 'About', path: '/about' },
  { name: 'Services', path: '/services' },
  { name: 'Contact', path: '/contact' }
]);

const toggleMenu = () => {
  showMenu.value = !showMenu.value;
};

// Close mobile menu when clicking outside
const clickOutside = (event) => {
  if (!event.target.closest('nav')) {
    showMenu.value = false;
  }
};

// Handle route changes
watch(() => route.path, () => {
  showMenu.value = false;
});

// Event listeners
onMounted(() => {
  document.addEventListener('click', clickOutside);
});

onUnmounted(() => {
  document.removeEventListener('click', clickOutside);
});
</script>

<style scoped>
/* Full-screen Slide Down Animation */
.fullscreen-slide-enter-active,
.fullscreen-slide-leave-active {
  transition: transform 0.4s ease-out, opacity 0.4s ease-out;
}

.fullscreen-slide-enter-from,
.fullscreen-slide-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
