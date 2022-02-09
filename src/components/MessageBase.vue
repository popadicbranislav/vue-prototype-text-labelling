<script setup lang="ts">
interface MessageObject {
  text: string;
  labels: {
    id: string;
    name: string;
    start: number;
    end: number;
  }[];
}

defineProps<{ value: MessageObject }>();
</script>

<template>
  <div>{{ value.text }}</div>
</template>

<style scoped></style>
