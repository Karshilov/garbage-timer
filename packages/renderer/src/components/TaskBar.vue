<script setup lang="ts">
import { onMounted, onUnmounted, ref } from "vue";

const currentTask = ref("");

const updateCurrent = () => {
  let task: any = localStorage.getItem("currentTask");
  let mode = localStorage.getItem("mode");
  if (mode === null && task) {
    if (JSON.parse(task).name.includes("#tomato")) {
      localStorage.removeItem("currentTask");
      currentTask.value = "";
      return;
    }
  }
  if (task === null) {
    if (!mode) {
      currentTask.value = "";
      return;
    } else {
      currentTask.value = "工作#tomato";
      localStorage.setItem(
        "currentTask",
        JSON.stringify({
          name: "工作#tomato",
          startTime: Number(new Date()),
          duration: 25 * 60 * 1000,
        })
      );
      return;
    }
  }
  task = JSON.parse(task);
  if (Number(new Date()) - task!.startTime > task.duration) {
    if (!mode) {
      currentTask.value = "";
      localStorage.removeItem("currentTask");
      return;
    } else {
      if (task.name === "休息#tomato") {
        currentTask.value = "工作#tomato";
        localStorage.setItem(
          "currentTask",
          JSON.stringify({
            name: "工作#tomato",
            startTime: Number(new Date()),
            duration: 25 * 60 * 1000,
          })
        );
        return;
      } else {
        currentTask.value = "休息#tomato";
        localStorage.setItem(
          "currentTask",
          JSON.stringify({
            name: "休息#tomato",
            startTime: Number(new Date()),
            duration: 5 * 60 * 1000,
          })
        );
        return;
      }
    }
  }
  else if (task.name !== currentTask) {
    currentTask.value = task.name;
  }
};

onMounted(() => {
  setInterval(updateCurrent, 1000 - new Date().getMilliseconds());
});

function createRipple(event: MouseEvent) {
  const button = event.currentTarget as HTMLElement;
  const circle = document.createElement("span");
  const diameter = Math.max(button.clientWidth, button.clientHeight);
  const radius = diameter / 2;
  circle.style.width = circle.style.height = `${diameter}px`;
  circle.style.left = `${event.clientX - (button.offsetLeft + radius)}px`;
  circle.style.top = `${event.clientY - (button.offsetTop + radius)}px`;
  circle.classList.add("ripple");
  const ripple = button.getElementsByClassName("ripple")[0];

  if (ripple) {
    ripple.remove();
  }
  button.appendChild(circle);
}

const setMode = () => {
  const mode = localStorage.getItem("mode");
  if (mode === null) {
    localStorage.setItem("mode", "true");
    return;
  } else localStorage.removeItem("mode");
};

onMounted(() => {
  const buttons = document.getElementsByTagName("button");
  for (let i = 0; i < buttons.length; i++) {
    let currentButton = buttons.item(i) as HTMLButtonElement;
    currentButton.addEventListener("click", createRipple);
  }
});
</script>

<template>
  <div class="taskbar">
    <span class="label">{{
      currentTask === "" ? "当前暂无任务" : currentTask.split("#")[0]
    }}</span>
    <button class="tomato" @click="setMode()">番茄 MODE</button>
  </div>
</template>

<style lang="scss">
@font-face {
  font-family: "SourceHanSansCN-Normal";
  src: url("../assets/SourceHanSansCN-Normal.ttf");
}
.taskbar {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  font-size: 16px;
  font-family: "SourceHanSansCN-Normal";
  padding-left: calc((100% - (7px + 7vmin) * 8) / 2);
  padding-right: calc((100% - (7px + 7vmin) * 8) / 2);
  box-sizing: border-box;

  .label {
    color: rgba(0, 0, 0, 0.7);
    padding-left: 2px;
    padding-right: 2px;
    font-size: 18px;
    background: linear-gradient(
      to top,
      rgba(252, 169, 15, 0.5),
      rgba(252, 169, 15, 0.5) 4px,
      transparent 4px,
      transparent
    );
  }

  .tomato {
    padding: 2px 5px;
    border-radius: 4px;
    background: #ffb11b;
    outline: none;
    color: #fff;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    border: none;
    box-shadow: 0 0 0.5rem rgba(203, 157, 6, 0.3);
  }
  span.ripple {
    position: absolute;
    border-radius: 50%;
    transform: scale(0);
    animation: ripple 600ms linear;
    background-color: rgba(255, 255, 255, 0.7);
  }
  @keyframes ripple {
    to {
      transform: scale(4);
      opacity: 0;
    }
  }
}
</style>
