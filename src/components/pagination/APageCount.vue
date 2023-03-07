<template>
  <label>
    전체 페이지 수 :
    <input type="number" v-model="PageCountValue" />
    <button @click="onClick(PageCountValue)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRefs } from "vue";

const PageCountValue = ref<number>();
const { modelValue } = toRefs(props);

const props = defineProps<{
  modelValue: number;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  console.log("pageCount value", value);
  emit("update:modelValue", value);
}
</script>

<style scoped></style>
