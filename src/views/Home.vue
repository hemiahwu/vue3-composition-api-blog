<script setup>
let name = "mr.wu";
let age = 30;

const handleClick = () => {
  name = "米斯特吴";
  console.log(name);
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
