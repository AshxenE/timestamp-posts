<template>
  <div class="post-list">
    <h2 class="heading">Список постов:</h2>
    <ul>
      <li v-for="post in posts" :key="post.id" class="post-item">
        <h3 class="post-title">{{ post.title }}</h3>
        <p class="post-body">{{ post.body }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useAsyncData } from '#app';

interface Post {
  id: number;
  title: string;
  body: string;
}

const { data: postData } = await useAsyncData<Post[]>('posts', async () => {
  const response = await $fetch<Post[]>('https://jsonplaceholder.typicode.com/posts');
  return response;
});

const posts = ref<Post[]>(postData.value || []);
</script>

<style scoped lang="scss">
.post-list {
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  .heading {
    font-size: 1.6em;
    color: #333;
    margin-bottom: 20px;
    text-align: center;
  }

  ul {
    list-style: none;
    padding: 0;
  }

  .post-item {
    margin-bottom: 20px;
    padding: 15px;
    background-color: #f9f9f9;
    border-radius: 5px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);

    .post-title {
      font-size: 1.4em;
      color: #007acc;
      margin-bottom: 10px;
    }

    .post-body {
      font-size: 1em;
      color: #555;
    }
  }
}
</style>
