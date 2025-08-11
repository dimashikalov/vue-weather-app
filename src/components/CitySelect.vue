<script setup>
import Button from './Button/Button.vue';
import IconLocation from '../icons/IconLocation.vue';
import Input from './Input/Input.vue';
import { inject, ref } from 'vue';
import { cityProvide } from '../constants';

const city = inject(cityProvide);
let isEdited = ref(false);
let inputValue = ref(city.value);

function select() {
  isEdited.value = false;
  city.value = inputValue.value;
}

function edit() {
  isEdited.value = true;
}
</script>
<template>
  <div class="city-select">
    <div v-if="isEdited" class="city-input">
      <Input
        v-focus
        placeholder="Введите город"
        v-model="inputValue"
        v-model:additional="city"
        @keyup.enter="select()"
      />
      <Button @click="select()">Сохранить</Button>
    </div>
    <div v-else>
      <Button @click="edit()">
        <IconLocation />
        Изменить город
      </Button>
    </div>
  </div>
</template>
<style scoped>
.city-select {
  width: 420px;
}
.city-input {
  display: flex;
  gap: 12px;
}
</style>
