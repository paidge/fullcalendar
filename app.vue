<script setup>
const date = ref(getToday());

function getToday() {
  const today = new Date();
  const month = today.getMonth() + 1;
  const month_f = month < 10 ? "0" + month : month;
  return today.getFullYear() + "-" + month_f + "-" + today.getDate();
}
</script>

<template>
  <div>
    <input type="date" v-model="date" />
    <Calendar :date="date" />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}

body {
  padding: 1rem;
}

input[type="date"] {
  margin-bottom: 1rem;
}
</style>
