<script setup>
const props = defineProps({
  error: String,
});
</script>
<template>
  <div class="error">
    {{ props.error }}
  </div>
</template>
<style scoped>
.error {
  font-size: 18px;
  padding: 15px;
  text-align: center;
  background: var(--color-bg-card);
  border-radius: 15px;
  margin-top: -50px;
  margin-bottom: 60px;
}
</style>
