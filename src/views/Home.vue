<script setup>
import { ref } from "vue";
let name = ref("mr.wu");
let age = ref(30);

const handleClick = () => {
  name.value = "米斯特吴";
};
</script>

<template>
  <div>
    <p>{{ name }} - {{ age }}</p>
    <button @click="handleClick">click me</button>
  </div>
</template>

<style>
</style>
