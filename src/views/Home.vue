<script setup>
import { ref, reactive } from "vue";

const refData = ref("mr.wu");
const reactiveData = reactive("米斯特吴");

const updateRefData = () => {
  refData.value = "mister.wu";
};

const updateReactiveData = () => {
  reactiveData = "米斯特吴123";
};
</script>

<template>
  <div class="home">
    <p>RefData: {{ refData }}</p>
    <button @click="updateRefData">更新RedData</button>

    <p>ReactiveData: {{ reactiveData }}</p>
    <button @click="updateReactiveData">更新ReactiveData</button>
  </div>
</template>

<style>
</style>
