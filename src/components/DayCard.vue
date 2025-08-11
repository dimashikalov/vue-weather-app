<script setup>
import IconSun from './../icons/IconSun.vue';
import IconRain from './../icons/IconRain.vue';
import IconCloud from './../icons/IconCloud.vue';
import { computed } from 'vue';

const { weatherCode, day, temperature, isActive } = defineProps({
  weatherCode: Number,
  day: Date,
  temperature: Number,
  isActive: Boolean,
});

const iconColor = computed(() => {
  return isActive ? 'var(--color-primary-inverted)' : 'var(--color-primary)';
});
</script>
<template>
  <button class="card" :class="{ active: isActive }">
    <div>
      <IconSun v-if="weatherCode <= 1003" :color="iconColor" />
      <IconCloud
        v-else-if="weatherCode >= 1006 && weatherCode < 1063"
        :color="iconColor"
      />
      <IconRain v-else-if="weatherCode >= 1063" :color="iconColor" />
    </div>
    <div>
      {{ day.toLocaleDateString('ru-Ru', { weekday: 'short' }) }}
    </div>
    <div class="temp">{{ temperature.toFixed() }} &#8451;</div>
  </button>
</template>
<style scoped>
.card {
  width: 103px;
  height: 165px;
  padding: 20px 24px;
  border-radius: 10px;
  box-shadow: 1px 2px 4px 0 var(--color-bg-main);
  background: var(--color-bg-card);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 15px;
  color: var(--color-primary);
  font-size: 20px;

  cursor: pointer;
  border: none;
}

.card:not(.active):hover {
  background: var(--color-bg-card-hover);
}

.active {
  background: var(--color-primary);
  color: var(--color-primary-inverted);
}

.temp {
  font-weight: 700;
}
</style>
