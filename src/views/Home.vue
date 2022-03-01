<script setup>
import PostList from "../components/PostList.vue";
import getPosts from '../composibles/getPosts'

const {posts,load} = getPosts()
load()

</script>

<template>
  <div class="home">
    <div v-if="posts.length">
      <PostList :posts="posts"  />
    </div>
    <div v-else>加载中....</div>
  </div>
</template>

<style>
</style>
