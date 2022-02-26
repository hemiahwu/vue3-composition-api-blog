<script setup>
import { computed } from "vue";

const props = defineProps({
  post: Object,
});

const snippet = computed(() => {
  return props.post.body.substring(0, 100) + "...";
});
</script>

<template>
  <div class="post">
    <h3>{{ post.title }}</h3>
    <p>{{ snippet }}</p>
  </div>
</template>


<style>
</style>