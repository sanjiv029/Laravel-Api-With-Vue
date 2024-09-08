<script setup>
import { usePostsStore } from "@/stores/posts";
import { storeToRefs } from "pinia";
import {onMounted, reactive } from "vue";


const {errors} = storeToRefs(usePostsStore());
const {createPost} = usePostsStore();

const formData = reactive({
  title: "",
  body: "",
});

const handleSubmit = () => {
  createPost(formData);
};

onMounted(() => (errors.value = {}));
</script>

<template>
    <main>
        <h1 class="title">Create a new post</h1>
        <form class="w-1/2 mx-auto space-y-4" @submit.prevent="handleSubmit">
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

            <button type="submit" class="primary-btn hover:bg-green-700 text-white font-bold py-2 px-4 rounded">Create</button>
        </form>
    </main>
</template>