<template>
  <div class="tag">
    <div v-if="posts.length">
      <PostList :posts="postsWithTag" />
      <TagCloud :posts="posts" />
    </div>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import getPosts from "../composibles/getPosts";
import { computed } from "vue";
import PostList from "../components/PostList.vue";
import TagCloud from "../components/TagCloud.vue";

const route = useRoute();
const { posts, load } = getPosts();
load();

const postsWithTag = computed(() => {
  return posts.value.filter((p) => p.tags.includes(route.params.tag));
});
</script>

<style scoped>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>