<script setup>
import { useAuthStore } from '@/stores/auth';
import { usePostsStore } from '@/stores/posts';
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const { getPost,deletePost } =usePostsStore();
const authStore = useAuthStore();
const post = ref(null);
onMounted(async()=> (post.value= await getPost( route.params.id))); 
</script>

<template>
    <main>
        <div v-if="post">
    <div  class="border-l-4 border-blue-500 pl-4 mt-12">
      <h2 class="font-bold text-3xl"> {{ post.title }}</h2>
      <p class="text-xs text-slate-600 mb-4">
        Posted by {{ post.user.name }}
      </p>
      <p>
        {{ post.body }}
    </p>
 
    <div v-if="authStore.user && authStore.user.id === post.user_id" 
    class="flex items-center gap-6">
        <form @submit.prevent="deletePost(post)">
        <button class="border border-red-400 hover:bg-red-700 text-black py-2 px-4 rounded mt-6">Delete</button>
        </form>
        <RouterLink 
        :to="{name: 'update',params: {id: post.id}}"
        class="border border-green-400 hover:bg-green-700 text-black py-2 px-4 rounded mt-6">
        Update
    </RouterLink>
    </div>
    </div>
 
  </div>
  <div v-else>
    <h2 class="title">Page not found</h2>
  </div>    </main>
</template>