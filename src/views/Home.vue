<script setup>
import { ref, reactive, computed } from "vue";

const names = ref([
  { name: "小猫" },
  { name: "小狗" },
  { name: "小猪" },
  { name: "小爱" },
]);
</script>

<template>
  <div class="home">
    <p v-for="(item, index) in names" :key="index">{{ item.name }}</p>
  </div>
</template>

<style>
</style>
