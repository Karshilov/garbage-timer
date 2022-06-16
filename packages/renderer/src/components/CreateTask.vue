<script setup lang="ts">
import { ref } from "vue";
import TimePicker from "./TimePicker.vue";
const duration = ref(0);
const name = ref("");

const setTask = () => {
  if (duration.value > 0 && name.value.length > 0) {
    const task = {
      name: name.value,
      duration: duration.value,
      startTime: Number(new Date()),
    };
    localStorage.setItem("currentTask", JSON.stringify(task));
    name.value = "";
    duration.value = 0;
  }
};
</script>

<template>
  <div class="info-container">
    <TimePicker @on-update="(v) => (duration = v)"></TimePicker>
    <input class="name" v-model="name" />
    <button class="submit" @click="setTask">开始</button>
  </div>
</template>

<style lang="scss">
.info-container {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  .name {
    width: 75%;
    border: none;
    border-bottom: 1px solid gray;
    outline: none;
    padding: 5px;
    font-size: 14px;
    font-family: "Quicksand";
    color: gray;
    margin-block: 15px;
  }
  .submit {
    border: none;
    outline: none;
    border-radius: 5px;
    padding: 8px;
    background-color: #ffb11b;
    color: white;
    font-size: 14px;
    font-family: "Quicksand";
    cursor: pointer;
  }
}
</style>
