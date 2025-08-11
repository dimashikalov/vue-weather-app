<script setup>
import { errorMap } from '../constants';
import CitySelect from './CitySelect.vue';
import DayCard from './DayCard.vue';
import Error from './Error.vue';
import Stat from './Stat/Stat.vue';
import { computed } from 'vue';

const { error, data, activeIndex } = defineProps({
  error: Object,
  data: Object,
  activeIndex: Number,
});

const emit = defineEmits(['select-index']);

const errorDisplay = computed(() => {
  return errorMap.get(error.error.code);
});

const statData = computed(() => {
  if (!data) return;
  const day = data.forecast.forecastday[activeIndex].day;
  return [
    {
      label: 'Влажность',
      stat: day.avghumidity + ' %',
    },
    {
      label: 'Вероятность дождя',
      stat: day.daily_chance_of_rain + ' %',
    },
    {
      label: 'Ветер',
      stat: day.avgvis_km + ' км/ч',
    },
  ];
});
</script>
<template>
  <Error v-if="error" :error="errorDisplay" />

  <div v-if="data">
    <div class="stats">
      <Stat v-for="item in statData" :key="item.label" v-bind="item" />
    </div>

    <div class="weather-cards">
      <DayCard
        v-for="(item, i) in data.forecast.forecastday"
        :key="item.date"
        :day="new Date(item.date)"
        :temperature="item.day.avgtemp_c"
        :weather-code="item.day.condition.code"
        :is-active="activeIndex == i"
        @click="() => emit('select-index', i)"
      />
    </div>
  </div>
  <CitySelect />
</template>
<style scoped>
.stats {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.weather-cards {
  display: flex;
  justify-content: center;
  gap: 10px;
  padding: 80px 0 70px 0;
}
</style>
