<script setup lang="ts">
import FlipCard from "./components/FlipCard.vue";
import { ref, computed, watch, onUnmounted } from "vue";

const targetTime = ref(new Date());
targetTime.value.setDate(targetTime.value.getDate() + 2);
targetTime.value.setHours(0, 0, 0, 0);

const currentTime = ref(new Date().getTime());
const oldDigit = ref(0);
const updateCurrentTime = () => {
  currentTime.value = new Date().getTime();
};
const timerId = setInterval(updateCurrentTime, 1000);

onUnmounted(() => {
  clearInterval(timerId);
});

const timeRemaining = computed(
  () => targetTime.value.getTime() - currentTime.value
);
const days = computed(() =>
  Math.floor(timeRemaining.value / (1000 * 60 * 60 * 24))
);
const hours = computed(() =>
  Math.floor((timeRemaining.value / (1000 * 60 * 60)) % 24)
);
const minutes = computed(() =>
  Math.floor((timeRemaining.value / (1000 * 60)) % 60)
);
const seconds = computed(() => Math.floor((timeRemaining.value / 1000) % 60));

const flipState = {
  days: ref(false),
  hours: ref(false),
  minutes: ref(false),
  seconds: ref(false),
};

const triggerFlip = (unit: keyof typeof flipState) => {
  flipState[unit].value = !flipState[unit].value;
  setTimeout(() => (flipState[unit].value = false), 1000);
};

watch(days, () => triggerFlip("days"));
watch(hours, () => triggerFlip("hours"));
watch(minutes, () => triggerFlip("minutes"));
watch(seconds, () => triggerFlip("seconds"));
</script>
<template>
  <div class="container">
    <h1>We're launching soon</h1>

    <div class="countdown-timer">
      <FlipCard
        :unit="'days'"
        :digit="days"
        :flip="flipState.days.value"
      />
      <FlipCard
        :unit="'hours'"
        :digit="hours"
        :flip="flipState.hours.value"
      />
      <FlipCard
        :unit="'minutes'"
        :digit="minutes"
        :flip="flipState.minutes.value"
      />

      <FlipCard
        :unit="'seconds'"
        :digit="seconds"
        :flip="flipState.seconds.value"
      />
    </div>
    <img src="/images/pattern-hills.svg" alt="" />
  </div>
</template>
<style scoped>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 5rem;
  justify-content: flex-start;
  align-items: center;
}
.countdown-timer {
  display: flex;
  gap: 1rem;
}
h1 {
  text-transform: uppercase;
  text-align: center;
}
img {
  position: absolute;
  bottom: 0;
  width: 100%;
}
</style>
