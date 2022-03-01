<script setup>
import getPost from "../composibles/getPost";
import Spinner from "../components/Spinner.vue";
import { useRoute } from "vue-router";

const route = useRoute();
const { post, load } = getPost(route.params.id);

load();

const props = defineProps({
  id: String,
});
</script>

<template>
  <div class="post" v-if="post">
    <h3>{{ post.title }}</h3>
    <p class="pre">{{ post.body }}</p>
  </div>
  <div v-else>
    <Spinner />
  </div>
</template>

<style>
.tags a {
  margin-right: 10px;
}
.post {
  max-width: 1200px;
  margin: 0 auto;
}
.post p {
  color: #444;
  line-height: 1.5em;
  margin-top: 40px;
}
.pre {
  white-space: pre-wrap;
}
</style>
