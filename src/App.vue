<script setup lang="ts">
import { reactive } from "vue";
import MessageBase from "./components/MessageBase.vue";

const message = reactive({
  text: "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Corporis, non!",
  entities: [
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
  <div class="content-wrapper">
    <MessageBase :value="message" />
  </div>
</template>

<style>
body {
  height: 100vh;
  width: 100vw;
}

*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  display: flex;
  align-items: center;

  background: hsl(0, 0%, 95%);
  height: 100%;

  color: hsl(0, 0%, 15%);
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.content-wrapper {
  background: #fff;
  max-width: min(100% - 1rem, 50rem);
  margin: auto;
  padding: 1rem;

  box-shadow: 0 1em 2em hsla(0, 0%, 0%, 0.15);
}
</style>
