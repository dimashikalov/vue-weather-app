<script setup>
import { computed } from 'vue';
import IconCloud from '../icons/IconCloud.vue';
import IconLocation from '../icons/IconLocation.vue';
import IconRain from '../icons/IconRain.vue';
import IconSun from '../icons/IconSun.vue';

const { city, data } = defineProps({
  city: String,
  data: Object,
});

const day = computed(() => {
  return new Date(data.date).toLocaleDateString('ru-Ru', {
    weekday: 'long',
  });
});

const date = computed(() => {
  return new Date(data.date).toLocaleDateString('ru-Ru', {
    day: 'numeric',
    month: 'long',
    year: 'numeric',
  });
});

const weatherCode = computed(() => {
  return data.day.condition.code;
});
</script>
<template>
  <div class="wrapper">
    <div class="date-info">
      <div class="week-day">
        {{ day }}
      </div>
      <div class="date">
        {{ date }}
      </div>
      <div class="location"><IconLocation /> {{ city }}</div>
    </div>
    <div class="weather-info">
      <div class="icon">
        <IconSun v-if="weatherCode <= 1003" :size="95" />
        <IconCloud
          v-else-if="weatherCode >= 1006 && weatherCode < 1063"
          :size="95"
        />
        <IconRain v-else-if="weatherCode >= 1063" :size="95" />
      </div>
      <div class="temp">{{ data.day.avgtemp_c.toFixed() }} &#8451;</div>
      <div class="condition">{{ data.day.condition.text }}</div>
    </div>
  </div>
</template>
<style scoped>
.wrapper {
  padding: 48px 32px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}
.date-info {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
.weather-info {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.icon {
  padding-left: 20px;
}
.week-day {
  text-transform: capitalize;
  font-weight: 700;
  font-size: 37px;
}
.date {
  font-weight: 500;
  font-size: 22px;
}
.location {
  gap: 8px;
  font-weight: 600;
  font-size: 20px;
}
.temp {
  font-weight: 700;
  font-size: 50px;
}
.condition {
  font-weight: 700;
  font-size: 30px;
}
</style>
