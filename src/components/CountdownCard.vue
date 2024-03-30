<script setup lang="ts">
const props = defineProps({
  days: Number,
  hours: Number,
  minutes: Number,
  seconds: Number,
});
</script>
<template>
  <div class="countdown">
    <div class="two-digit" v-if="days !== undefined && days < 10">
      {{ "0" + days }}
    </div>
    <div class="two-digit" v-else>{{ days }}</div>
    <div class="two-digit" v-if="hours !== undefined && hours < 10">
      {{ "0" + hours }}
    </div>
    <div class="two-digit" v-else>{{ hours }}</div>
    <div class="two-digit" v-if="minutes !== undefined && minutes < 10">
      {{ "0" + minutes }}
    </div>
    <div class="two-digit" v-else>{{ minutes }}</div>
    <div class="two-digit" v-if="seconds !== undefined && seconds < 10">
      {{ "0" + seconds }}
    </div>
    <div class="two-digit" v-else>{{ seconds }}</div>
  </div>
</template>
<style scoped>
.countdown {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
}

.two-digit {
  font-size: 5rem;
  text-align: center;
  font-weight: 700;
  color: var(--vt-c-accent);
  background-color: var(--vt-c-primary);
  height: 5rem;
  border-radius: 0.5rem;
  padding: 0.5rem;
}

</style>
