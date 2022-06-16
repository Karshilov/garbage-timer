<script setup lang="ts">
const clearTask = () => {
  localStorage.removeItem("currentTask");
};
</script>

<template>
  <button class="clear-task" @click="clearTask">取消任务</button>
</template>

<style lang="scss">
.clear-task {
  border: none;
  outline: none;
  border-radius: 5px;
  padding: 8px;
  background-color: #ffb11b;
  color: white;
  font-size: 14px;
  font-family: "Quicksand";
  cursor: pointer;
  margin-top: 20px;
}
</style>
