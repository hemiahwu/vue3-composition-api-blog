<script setup>
import { ref, reactive, computed } from "vue";

const name = computed(() => {
  return "米斯特吴";
});
</script>

<template>
  <div class="home">
    <h1>{{ name }}</h1>
  </div>
</template>

<style>
</style>
