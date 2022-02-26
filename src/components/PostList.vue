<script setup>
import SinglePost from "./SinglePost.vue";
defineProps({
  posts: Array,
});
</script>

<template>
  <div class="post-list">
    <div v-for="post in posts" :key="post.id">
      <SinglePost :post="post" />
    </div>
  </div>
</template>

<style>
</style>
