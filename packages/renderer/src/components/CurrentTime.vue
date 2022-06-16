<script setup lang="ts">
import { onMounted } from "vue";

function digit(num: number, first: boolean) {
  if (first) return "<div>" + num + "</div>";
  return '<div class="fade-in">' + num + "</div>";
}

const prev = new Map<string, number>();

function set(id: string, content: number) {
  if (prev.get(id) === content) return;
  document.getElementById(id)!.innerHTML = digit(
    content,
    prev.get(id) === undefined
  );
  prev.set(id, content);
}

function updateTime() {
  let date = new Date();
  set("hour1", parseInt((date.getHours() / 10).toString()));
  set("hour2", date.getHours() % 10);
  set("minute1", parseInt((date.getMinutes() / 10).toString()));
  set("minute2", date.getMinutes() % 10);
  set("second1", parseInt((date.getSeconds() / 10).toString()));
  set("second2", date.getSeconds() % 10);
  setTimeout(function () {
    updateTime();
  }, 1000 - date.getMilliseconds());
}

onMounted(() => {
  updateTime();
});
</script>

<template>
  <div>
    <span class="current-label">现在是</span>
    <div id="clock">
      <div class="digit" id="hour1"></div>
      <div class="digit" id="hour2"></div>
      <div class="digit" id="separator1">:</div>
      <div class="digit" id="minute1"></div>
      <div class="digit" id="minute2"></div>
      <div class="digit" id="separator2">:</div>
      <div class="digit" id="second1"></div>
      <div class="digit" id="second2"></div>
    </div>
  </div>
</template>

<style lang="scss">
@font-face {
  font-family: "Quicksand";
  src: url("../assets/Quicksand-Light.ttf");
}

@font-face {
  font-family: "SourceHanSansCN-Normal";
  src: url("../assets/SourceHanSansCN-Normal.ttf");
}

.current-label {
  font-size: 24px;
  font-family: "SourceHanSansCN-Normal";
  color: rgba(0, 0, 0, 0.7);
  padding-left: 4px;
  padding-right: 4px;
  background: linear-gradient(
    to top,
    transparent,
    transparent 2px,
    rgba(252, 169, 15, 0.5) 2px,
    rgba(252, 169, 15, 0.5) 8px,
    transparent 8px,
    transparent
  );
}

#clock {
  display: flex;
  flex-direction: row;
  font-size: calc(10px + 10vmin);
  font-family: "Quicksand";
  color: gray;
  margin-top: 10px;

  .digit {
    display: inline-block;
    width: calc(7px + 7vmin);
    transition: color 1s, transform 1s;
  }

  .digit:hover {
    color: black;
    transform: scale(1.1);
  }

  .fade-in {
    animation: fadeinkf 0.3s ease-out;
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
}
</style>
