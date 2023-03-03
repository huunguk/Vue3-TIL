<script setup lang="ts">
import APagination from "./components/pagination/APagination.vue";
import InputNumber from "./components/pagination/InputNumber.vue";
</script>

<template>
  <APagination :show-length="8" :total="20" />
  <InputNumber />
</template>

<style scoped></style>
