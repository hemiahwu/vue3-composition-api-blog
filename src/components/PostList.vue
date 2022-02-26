<script setup>
defineProps({
  posts: Array,
});
</script>

<template>
  <div class="post-list">
    <div v-for="post in posts" :key="post.id">
      <h3>{{ post.title }}</h3>
    </div>
  </div>
</template>

<style>
</style>
