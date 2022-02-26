<script setup>
import { ref, reactive } from "vue";

const refData = ref({ name: "mr.wu", age: 30 });
const reactiveData = reactive({ name: "米斯特吴", age: 28 });

const updateRefData = () => {
  refData.value.name = "mister.wu";
};

const updateReactiveData = () => {
  reactiveData.name = "米斯特吴123";
};
</script>

<template>
  <div class="home">
    <p>RefData: {{ refData.name }} - {{ refData.age }}</p>
    <button @click="updateRefData">更新RedData</button>

    <p>ReactiveData: {{ reactiveData.name }} - {{ reactiveData.age }}</p>
    <button @click="updateReactiveData">更新ReactiveData</button>
  </div>
</template>

<style>
</style>
