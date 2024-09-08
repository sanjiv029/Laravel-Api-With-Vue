<script setup>
import { useAuthStore } from "@/stores/auth";
import { usePostsStore } from "@/stores/posts";
import { storeToRefs } from "pinia";
import {onMounted, reactive, ref } from "vue";
import { useRoute, useRouter } from "vue-router";

const router = useRouter();
const route = useRoute();

const {user} = storeToRefs(useAuthStore());
const {errors} = storeToRefs(usePostsStore());
const {getPost,updatePost} = usePostsStore();
const post = ref(null);
const formData = reactive({
  title: "",
  body: "",
});

onMounted(async() => {
    post.value = await getPost(route.params.id);
    if ( user.value.id !== post.value.user_id) {
        router.push({name: "home"})
    } else {
        formData.title = post.value.title;
        formData.body = post.value.body;
    }
  
});
</script>

<template>
    <main>
        <h1 class="title">Update your new post</h1>
        <form  @submit.prevent="updatePost(post, formData)" class="w-1/2 mx-auto space-y-4">
            <div>
                <fieldset>Post Title</fieldset>
                <p v-if="errors.title" class="error">{{ errors.title[0] }}</p>
                <input type="text" v-model="formData.title" >
            </div>
            <div>
                <fieldset>Post Content</fieldset>
                <p v-if="errors.body" class="error">{{ errors.body[0] }}</p>
                <textarea rows="10" v-model="formData.body"></textarea>
            </div>
            <button class="primary-btn hover:bg-green-700 text-white font-bold py-2 px-4 rounded">Update</button>
        </form>
    </main>
</template>