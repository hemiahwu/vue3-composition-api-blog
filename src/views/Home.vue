<script setup>
import { ref, reactive, computed } from "vue";

const names = ref([
  { name: "小猫" },
  { name: "小狗" },
  { name: "小猪" },
  { name: "小爱" },
]);

const search = ref("米斯特吴");
</script>

<template>
  <div class="home">
    <input type="text" v-model="search" />
    <!--input textarea select -->
    <p>{{ search }}</p>
    <p v-for="(item, index) in names" :key="index">{{ item.name }}</p>
  </div>
</template>

<style>
</style>
