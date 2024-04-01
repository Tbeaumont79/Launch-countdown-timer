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
  box-shadow: 0 2px 3px 0 rgba(0, 0, 0, 0.2);
  display: inline-flex;
  flex-direction: column;
  border-radius: 0.1em;
  font-size: 3em;
  color: var(--vt-c-accent);
}

.card-face {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2em;
}
.card-upper,
.card-lower,
.flip-card .card-upper-flip,
.flip-card.flip .card-lower-flip {
  height: 0.75em;
  align-self: center;
  line-height: 1;
  padding: 0.5em;
  overflow: hidden;
}
.card-upper,
.flip-card .card-upper-flip {
  border-top-left-radius: 0.1em;
  border-top-right-radius: 0.1em;
  background-color: #2d2d45;
}

.card-upper {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.card-lower,
.flip-card .card-lower-flip {
  background-color: #34364f;
  display: flex;
  align-items: flex-end;
  border-bottom-left-radius: 0.1em;
  border-bottom-right-radius: 0.1em;
}
.flip-card .card-upper-flip {
  position: absolute;
  text-align: center;
  overflow: hidden;
  transform-origin: bottom;
  animation: flip-upper 250ms ease-in;
}
.flip-card .card-lower-flip {
  position: absolute;
  bottom: 0;
  text-align: center;
  animation: flip-lower 250ms ease-out 250ms;
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
