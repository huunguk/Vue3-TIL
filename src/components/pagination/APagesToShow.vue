<template>
  <label>
    페이지네이션 길이 :
    <input type="number" v-model="InputPageValue" />
    <button @click="onClick(InputPageValue)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRefs } from "vue";

const InputPageValue = ref<number>();
const { modelValue } = toRefs(props);

const props = defineProps<{
  modelValue: number;
}>();

const emits = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  console.log("InputPageValue value", value);
  emits("update:modelValue", value);
}
</script>

<style scoped></style>
