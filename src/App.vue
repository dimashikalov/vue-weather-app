<script setup>
import { onMounted, provide, ref, watch } from 'vue';
import PanelRight from './components/PanelRight.vue';
import { API_ENDPOINT, cityProvide } from './constants';
import PanelLeft from './components/PanelLeft.vue';

let data = ref();
let errorData = ref();
const activeIndex = ref(0);
const city = ref('Москва');

provide(cityProvide, city);

watch(city, () => {
  getCity(city.value);
});
onMounted(() => {
  getCity(city.value);
});

async function getCity(city) {
  const apiKey = import.meta.env.VITE_APP_WEATHER_API_KEY;
  const params = new URLSearchParams({
    q: city,
    lang: 'ru',
    key: apiKey,
    days: 3,
  });

  console.log('key ', import.meta.env.VITE_APP_WEATHER_API_KEY);
  const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`);

  if (!res.ok) {
    errorData.value = await res.json();
    data.value = null;
    return;
  }
  errorData.value = null;
  data.value = await res.json();
}
</script>

<template>
  <main class="main">
    <div class="left">
      <PanelLeft
        v-if="data"
        :city
        :data="data.forecast.forecastday[activeIndex]"
      />
    </div>
    <div class="right">
      <PanelRight
        :data="data"
        :error="errorData"
        :active-index="activeIndex"
        @select-index="(i) => (activeIndex = i)"
      />
    </div>
  </main>
</template>

<style scoped>
.main {
  display: flex;
  align-items: center;
  justify-content: center;
}
.left {
  width: 500px;
  height: 680px;
  border-radius: 30px;
  background-image: url('/public/bg.png');
  background-repeat: no-repeat;
  background-size: cover;
}
.right {
  background-color: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 0 25px 25px 0;
}
</style>
