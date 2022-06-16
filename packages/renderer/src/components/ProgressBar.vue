<script setup lang="ts">
import { ref, onMounted } from "vue";
const progress = ref(0);
const total = ref(0);
const startTime = ref(0);
const currentRemain = ref({
  hour: 0,
  minute: 0,
  second: 0,
});
const updateProgress = () => {
  const raw = localStorage.getItem("currentTask");
  if (!raw) {
    currentRemain.value = {
      hour: 0,
      minute: 0,
      second: 0,
    };
    return;
  }
  const task = JSON.parse(raw);
  total.value = task.duration;
  startTime.value = task.startTime;
  progress.value = ((Number(new Date()) - task.startTime) / total.value) * 100;
  const hour = Math.floor(
    (startTime.value + total.value - Number(new Date())) / 1000 / 60 / 60
  );
  const minute = Math.floor(
    (startTime.value +
      total.value -
      Number(new Date()) -
      hour * 1000 * 60 * 60) /
      1000 /
      60
  );
  const second = Math.floor(
    (startTime.value +
      total.value -
      Number(new Date()) -
      hour * 1000 * 60 * 60 -
      minute * 1000 * 60) /
      1000
  );
  currentRemain.value = {
    hour,
    minute,
    second,
  };
  const $circle = document.querySelector("#svg #bar") as SVGCircleElement;
  if ($circle === null) return;

  const r = parseFloat($circle.getAttribute("r") || "0");
  const c = Math.PI * (r * 2);

  if (progress.value < 0) {
    progress.value = 0;
  }
  if (progress.value > 100) {
    progress.value = 100;
  }

  const pct = Math.floor((progress.value / 100) * c);
  console.log(pct, progress.value);

  $circle.style.strokeDashoffset = pct.toString();

  document.querySelector("#cont")?.setAttribute("data-pct", pct.toFixed(0));
};

onMounted(() => {
  setInterval(updateProgress, 1000 - new Date().getMilliseconds());
});
</script>

<template>
  <div class="progress-container">
    <div id="cont" data-pct="100">
      <div id="remain-time">
        {{
          `${Math.floor(currentRemain.hour / 10)}${
            currentRemain.hour % 10
          }:${Math.floor(currentRemain.minute / 10)}${
            currentRemain.minute % 10
          }:${Math.floor(currentRemain.second / 10)}${
            currentRemain.second % 10
          }`
        }}
      </div>
      <svg
        id="svg"
        width="100"
        height="100"
        viewPort="0 0 50 50"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
      >
        <circle
          r="45"
          cx="50"
          cy="50"
          fill="transparent"
          stroke-dasharray="282.74"
          stroke-dashoffset="0"
        ></circle>
        <circle
          id="bar"
          r="45"
          cx="50"
          cy="50"
          fill="transparent"
          stroke-dasharray="282.74"
          stroke-dashoffset="0"
        ></circle>
      </svg>
    </div>
  </div>
</template>

<style lang="scss">
@font-face {
  font-family: "Quicksand";
  src: url("../assets/Quicksand-Light.ttf");
}

.progress-container {
  width: 100%;
  padding-left: calc((100% - (7px + 7vmin) * 8) / 2);
  padding-right: calc((100% - (7px + 7vmin) * 8) / 2);
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  margin-top: 20px;
}
#svg circle {
  stroke-dashoffset: 0;
  transition: stroke-dashoffset 1s linear;
  stroke: #f8dfb2;
  stroke-width: 0.5rem;
}
#svg #bar {
  stroke: #ff9f1e;
}
#cont {
  display: block;
  height: 100px;
  width: 100px;
  margin: 0.5em auto;
  box-shadow: 0 0 0.5rem rgba(203, 157, 6, 0.3);
  border-radius: 100%;
  position: relative;
}
#remain-time {
  position: absolute;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Quicksand";
  font-size: 1.2rem;
}
</style>
