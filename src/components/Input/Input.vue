<script setup>
const data = defineModel({ type: String, required: true, default: 'ooo' });
const additional = defineModel('additional', { type: String });
</script>

<template>
  <input class="input" v-model="data" />
</template>

<style scoped>
.input {
  width: 260px;
  height: 53px;
  border: none;
  border-radius: 10px;
  background-color: var(--color-bg-card);
  padding: 15px 17px;
  color: var(--color-primary);
  box-shadow: 1px 2px 4px 0 var(--color-bg-main);
  font-size: 18px;
}

.input::placeholder {
  color: var(--color-text-placeholder);
}
</style>
