<script setup lang="ts">
import { ref, watchEffect } from "vue";
const { radix } = defineProps<{
  radix: number;
}>();
const emit = defineEmits<{
  (e: "on-update", value: number): void;
}>();
const digit = ref(0);
watchEffect(() => {
  emit("on-update", digit.value);
});
</script>

<template>
  <div class="editable-number">
    <div
      class="increase-arrow"
      v-on:click="digit = (digit + 1) % radix"
    ></div>
    <div class="number-container">
      <div class="digit">
        {{ parseInt("" + digit / 10) }}
      </div>
      <div class="digit">{{ digit % 10 }}</div>
    </div>
    <div class="decrease-arrow" v-on:click="digit = (digit - 1 + radix) % radix"></div>
  </div>
</template>

<style lang="scss">
.editable-number {
  display: flex;
  flex-direction: column;
  align-items: center;
  .decrease-arrow {
    width: 0;
    height: 0;
    border-left: 7px solid transparent;
    border-right: 7px solid transparent;
    border-top: 7px solid rgb(195, 195, 195);
  }
  .decrease-arrow:hover {
    transform: scale(1.1);
    border-top-color: black;
    cursor: pointer;
  }
  .increase-arrow {
    width: 0;
    height: 0;
    border-left: 7px solid transparent;
    border-right: 7px solid transparent;
    border-bottom: 7px solid rgb(195, 195, 195);
  }
  .increase-arrow:hover {
    transform: scale(1.1);
    border-bottom-color: black;
    cursor: pointer;
  }
  .number-container {
    display: flex;
    flex-direction: row;
    margin-top: 7px;
    margin-bottom: 7px;
  }
}
.digit {
  display: inline-block;
  width: calc(7px + 7vmin);
  transition: color 1s, transform 1s;
  animation: fadeinkf 0.3s ease-out;
}

.digit:hover {
  color: black;
  transform: scale(1.1);
}

@keyframes fadeinkf {
  from {
    transform: translate(0, 1vh);
    opacity: 0%;
  }
  to {
    transform: translate(0, 0);
    opacity: 100%;
  }
}
</style>
