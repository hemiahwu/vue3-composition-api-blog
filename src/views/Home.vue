<script setup>
import { ref, reactive, computed } from "vue";

const names = ref([
  { name: "小猫" },
  { name: "小猫猫" },
  { name: "小狗" },
  { name: "小猪" },
  { name: "小爱" },
]);

const search = ref("");

const matchNames = computed(() => {
  return names.value.filter((item) => item.name.includes(search.value));
});
</script>

<template>
  <div class="home">
    <input type="text" v-model="search" />

    <p v-for="(item, index) in matchNames" :key="index">{{ item.name }}</p>
  </div>
</template>

<style>
</style>
