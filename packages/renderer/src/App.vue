<script setup lang="ts">
import CurrentTime from "./components/CurrentTime.vue";
import TaskBar from "./components/TaskBar.vue";
import ProgressBar from "./components/ProgressBar.vue";
import CreateTask from "./components/CreateTask.vue";
import ClearButton from "./components/ClearButton.vue";
import { onMounted, ref, Fragment } from "vue";

const hasTask = ref(false);

onMounted(() => {
  setInterval(() => {
    if (localStorage.getItem("currentTask") === null) hasTask.value = false;
    else hasTask.value = true;
  }, 1000 - new Date().getMilliseconds());
});
</script>

<template>
  <CurrentTime></CurrentTime>
  <div id="task-container">
    <TaskBar></TaskBar>
    <component :is="hasTask ? ProgressBar : CreateTask"></component>
    <component :is="hasTask ? ClearButton : null"></component>
  </div>
</template>

<style>
#app {
  margin: 0;
  padding: 15vh, 0, 0, 15vh;
  min-height: 70vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
#task-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 30vh;
  width: 100%;
}
</style>
