<script setup lang="ts">
const props = defineProps({
  unit: String,
  digit: Number,
  flip: Boolean,
  oldDigit: Number,
});

const padNumber = (num: number) => num.toString().padStart(2, "0");
</script>

<template>
  <div class="flip-card flip">
    <div class="card-upper" :key="unit + 'Upper' + digit">
      <div class="card-face">
        <p class="digit">{{ digit != undefined ? padNumber(digit) : "" }}</p>
      </div>
    </div>
    <div class="card-lower" :key="unit + 'Lower' + digit">
      <div class="card-face">
        <p class="digit">{{ digit != undefined ? padNumber(digit) : "" }}</p>
      </div>
    </div>
    <div
      class="card-upper-flip"
      :class="{ flip: flip }"
      :key="unit + 'Upper' + oldDigit"
      :digit="oldDigit"
    >
      <div class="card-face">
        <p class="digit">
          {{ oldDigit != undefined ? padNumber(oldDigit) : "" }}
        </p>
      </div>
    </div>
    <div
      class="card-lower-flip"
      :class="{ flip: flip }"
      :key="unit + 'Lower' + oldDigit"
      :digit="oldDigit"
    >
      <div class="card-face">
        <p class="digit">
          {{ oldDigit != undefined ? padNumber(oldDigit) : "" }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.flip-card {
  position: relative;
  display: inline-flex;
  flex-direction: column;
  justify-content: flex-start;
  letter-spacing: 0.05em;
  font-size: 3em;
  color: var(--vt-c-accent);
  background-color: hsl(240, 28%, 15%);
  border-bottom: 5px solid rgba(255, 255, 255, 0.2);
  overflow: hidden;
  cursor: pointer;
  border-radius: 0.1em;
}

.card-face {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2em;
  backface-visibility: hidden;
}

.card-upper,
.card-lower,
.flip-card .card-upper-flip,
.flip-card .card-lower-flip {
  align-self: center;
  line-height: 1;
  height: 0.5em;
  padding: 0.5em;
  overflow: hidden;
}
.card-upper,
.flip-card .card-upper-flip {
  border-bottom-left-radius: 0.4em;
  border-bottom-right-radius: 0.4em;
  background-color: #2d2d45;
  filter: opacity(0.8);
}

.card-lower,
.flip-card .card-lower-flip {
  background-color: #34364f;
  display: flex;
  border-top-left-radius: 0.4em;
  border-top-right-radius: 0.4em;
  align-items: flex-end;
}
.flip-card .card-upper-flip {
  position: absolute;
  text-align: center;
  overflow: hidden;
  transform-origin: bottom;
  animation: flip-upper 300ms ease-in;
}
.flip-card .card-lower-flip {
  position: absolute;
  bottom: 0;
  text-align: center;
  animation: flip-lower 300ms ease-out 300ms;
  transform-origin: top;
  transform: rotateX(-90deg);
}
.card-face .digit {
  font-weight: bold;
}

@keyframes flip-upper {
  100% {
    transform: rotateX(90deg);
  }
}

@keyframes flip-lower {
  100% {
    transform: rotateX(0deg);
  }
}
</style>
