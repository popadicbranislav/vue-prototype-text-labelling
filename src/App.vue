<script setup lang="ts">
import { reactive } from "vue";
import MessageBase from "./components/MessageBase.vue";

const message = reactive({
  text: "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Corporis, non!",
  labels: [
    {
      id: "bca6c3ab-bb3e-4921-a10a-002c0b0c868e",
      name: "New label",
      start: 0,
      end: 5,
    },
  ],
});
</script>

<template>
  <MessageBase :value="message" />
</template>

<style>
#app {
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #262626;
}
</style>
