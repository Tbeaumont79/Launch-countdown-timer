<script setup lang="ts">
import FlipCard from "./components/FlipCard.vue";
import { ref, computed, watch, onUnmounted } from "vue";

const targetTime = ref(new Date());
targetTime.value.setDate(targetTime.value.getDate() + 2);
targetTime.value.setHours(0, 0, 0, 0);

const currentTime = ref(new Date().getTime());

// Initialisation des références pour les anciennes valeurs
const oldDays = ref(0);
const oldHours = ref(0);
const oldMinutes = ref(0);
const oldSeconds = ref(0);

const updateCurrentTime = () => {
  // Stockage des valeurs actuelles avant la mise à jour
  oldDays.value = days.value;
  oldHours.value = hours.value;
  oldMinutes.value = minutes.value;
  oldSeconds.value = seconds.value;

  currentTime.value = new Date().getTime();
};

const timerId = setInterval(updateCurrentTime, 100);

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
  setTimeout(() => (flipState[unit].value = false), 250);
};

// Utilisation des watch pour déclencher les animations en fonction des anciennes valeurs
watch(days, (newVal, oldVal) => {
  if (newVal !== oldVal) triggerFlip("days");
});
watch(hours, (newVal, oldVal) => {
  if (newVal !== oldVal) triggerFlip("hours");
});
watch(minutes, (newVal, oldVal) => {
  if (newVal !== oldVal) triggerFlip("minutes");
});
watch(seconds, (newVal, oldVal) => {
  if (newVal !== oldVal) triggerFlip("seconds");
});
</script>
<template>
  <div class="container">
    <h1>We're launching soon</h1>

    <div class="countdown-timer">
      <div class="time-card">
        <FlipCard
          :unit="'days'"
          :digit="days"
          :oldDigit="oldDays"
          :flip="flipState.days.value"
        />
        <p>Days</p>
      </div>
      <div class="time-card">
        <FlipCard
          :unit="'hours'"
          :digit="hours"
          :oldDigit="oldHours"
          :flip="flipState.hours.value"
        />
        <p>Hours</p>
      </div>
      <div class="time-card">
        <FlipCard
          :unit="'minutes'"
          :digit="minutes"
          :oldDigit="oldMinutes"
          :flip="flipState.minutes.value"
        />
        <p>Minutes</p>
      </div>
      <div class="time-card">
        <FlipCard
          :unit="'seconds'"
          :digit="seconds"
          :oldDigit="oldSeconds"
          :flip="flipState.seconds.value"
        />
        <p>Seconds</p>
      </div>
    </div>
    <img src="/images/pattern-hills.svg" alt="" />
  </div>
</template>
<style scoped>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2rem;
  align-items: center;
}
.countdown-timer {
  display: flex;
  gap: 2rem;
}
.time-card {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: center;
  justify-content: center;
}
.time-card p {
  font-size: 0.9rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5em;
  color: var(--vt-c-primary);
  filter: opacity(0.8);
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
