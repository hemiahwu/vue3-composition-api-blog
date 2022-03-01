<template>
  <div class="tag">
    <div v-if="posts.length">
      <PostList :posts="postsWithTag" />
    </div>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import getPosts from "../composibles/getPosts";
import { computed } from "vue";
import PostList from "../components/PostList.vue";

const route = useRoute();
const { posts, load } = getPosts();
load();

const postsWithTag = computed(() => {
  return posts.value.filter((p) => p.tags.includes(route.params.tag));
});
</script>

<style>
</style>