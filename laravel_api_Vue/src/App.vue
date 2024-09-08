<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { useAuthStore } from './stores/auth';

const authStore = useAuthStore();

</script>

<template>
  <header>
      <nav>
        <RouterLink :to="{ name:'home' }" class="nav-link ">Home</RouterLink>
        <div v-if="authStore.user" class="flex items-center space-x-6">
        <p class="text-sm text-slate-300">
          Welcome back {{ authStore.user.name }}
        </p>
        <router-link :to="{name: 'create'}" class="nav-link">
          New Post
        </router-link>
          <form @submit.prevent="authStore.logout">
            <button  class="nav-link hover:bg-red-700 text-white font-bold py-2 px-4 rounded">Logout</button>
           </form>
        </div>
    
      <div v-else class="space-x-6">
        <RouterLink :to="{ name:'Register' }" class="nav-link hover:bg-blue-700 text-white font-bold py-2 px-4 rounded ">Register</RouterLink>
      
        <RouterLink :to="{ name:'Login' }" class="nav-link hover:bg-green-700 text-white font-bold py-2 px-4 rounded ">Login</RouterLink>
        </div>
      
    </nav>
  </header>

  <RouterView />
</template>

