<script setup>
import { ref, reactive, computed, watch, watchEffect } from "vue";

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

// watch([search, names], ([newSearch, newNames], [prevSearch, prevNames]) => {
//   console.log("watch函数触发了", newSearch, prevSearch);
// });

// watchEffect(() => {
//   console.log("watchEffect执行了", search.value);
// });

const stopWatch = watch(
  [search, names],
  ([newSearch, newNames], [prevSearch, prevNames]) => {
    console.log("watch函数触发了", newSearch, prevSearch);
  }
);

const stopEffect = watchEffect(() => {
  console.log("watchEffect执行了", search.value);
});

const handleClick = () => {
  stopWatch();
  stopEffect();
};
</script>

<template>
  <div class="home">
    <input type="text" v-model="search" />

    <button @click="handleClick">停止监听</button>

    <p v-for="(item, index) in matchNames" :key="index">{{ item.name }}</p>
  </div>
</template>

<style>
</style>
