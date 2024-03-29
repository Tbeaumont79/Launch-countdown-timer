<script setup lang="ts">
import { computed, ref, watch } from "vue";

const currentTime = ref(new Date().getTime());
const targetDate = new Date();
targetDate.setDate(targetDate.getDate() + 2);
targetDate.setHours(0, 0, 0, 0);

const timeRemaining = computed(() => {
  return targetDate.getTime() - currentTime.value > 0
    ? targetDate.getTime() - currentTime.value
    : 0;
});
const days = computed(() => {
  return Math.floor(timeRemaining.value / (1000 * 60 * 60 * 24));
});
const hours = computed(() => {
  return Math.floor(
    (timeRemaining.value % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
  );
});
const minutes = computed(() => {
  return Math.floor((timeRemaining.value % (1000 * 60 * 60)) / (1000 * 60));
});
const seconds = computed(() => {
  return Math.floor((timeRemaining.value % (1000 * 60)) / 1000);
});

const interval = setInterval(() => {
  currentTime.value = new Date().getTime();
}, 1000);
watch(
  () => timeRemaining.value,
  (newValue) => {
    if (newValue === 0) {
      clearInterval(interval);
    }
  }
);
</script>
<template>
  <h1>
    days : {{ days }} hours : {{ hours }} minutes : {{ minutes }} seconds :
    {{ seconds }}
  </h1>
</template>
