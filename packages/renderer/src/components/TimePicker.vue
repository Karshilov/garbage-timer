<script setup lang="ts">
import { ref, watchEffect } from "vue";
import ChangeableDigit from "./ChangeableDigit.vue";
const emit = defineEmits<{
  (e: "on-update", value: number): void;
}>();
const hour = ref(0);
const minute = ref(0);
const second = ref(0);
watchEffect(() => {
  emit(
    "on-update",
    hour.value * 60 * 60 * 1000 + minute.value * 60 * 1000 + second.value * 1000
  );
});
</script>

<template>
  <div id="time-picker">
    <ChangeableDigit
      :radix="24"
      @on-update="(v) => (hour = v)"
    ></ChangeableDigit>
    <div class="digit">:</div>
    <ChangeableDigit
      :radix="60"
      @on-update="(v) => (minute = v)"
    ></ChangeableDigit>
    <div class="digit">:</div>
    <ChangeableDigit
      :radix="60"
      @on-update="(v) => (second = v)"
    ></ChangeableDigit>
  </div>
</template>

<style lang="scss">
@font-face {
  font-family: "Quicksand";
  src: url("../assets/Quicksand-Light.ttf");
}

#time-picker {
  display: flex;
  flex-direction: row;
  font-size: calc(10px + 10vmin);
  font-family: "Quicksand";
  align-items: center;
  color: gray;
}
.digit {
  display: inline-block;
  width: calc(7px + 7vmin);
}
</style>
