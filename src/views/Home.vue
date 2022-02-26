<script setup>
import PostList from "../components/PostList.vue";
import { ref, reactive, computed, watch, watchEffect } from "vue";

const posts = ref([
  { title: "跟米斯特吴学习全新的Vue.js", body: "vue", id: 1 },
  { title: "跟米斯特吴学习全新的React.js", body: "react", id: 2 },
]);
</script>

<template>
  <div class="home">
    <PostList :posts="posts" />
  </div>
</template>

<style>
</style>
