<script setup>
import PostList from "../components/PostList.vue";
import { ref, reactive, computed, watch, watchEffect } from "vue";
import axios from 'axios'

const posts = ref([]);

const load = async () => {
  try {
    let {data} = await axios("http://localhost:3003/posts")
    posts.value = data
  } catch (error) {
    console.log(error)
  }
}

load()

const showPosts = ref(true);
</script>

<template>
  <div class="home">
    <div v-if="posts.length">
      <PostList :posts="posts" v-if="showPosts" />
    </div>
    <div v-else>加载中....</div>
  </div>
</template>

<style>
</style>
