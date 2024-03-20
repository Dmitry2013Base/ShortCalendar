<script setup>
  import Calendar from "@/components/Calendar.vue";
</script>

<template>
  <main>
    <Calendar/>
  </main>
</template>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  main {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
