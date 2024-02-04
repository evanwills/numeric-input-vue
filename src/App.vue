<script setup>
import NumericInput from './components/NumericInput.vue'
</script>

<template>
  <label for="blah">Test numeric input</label>
  <NumericInput field-id="blah" step="0.1" min="0" max="4" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
