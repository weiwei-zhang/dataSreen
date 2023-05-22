<script setup>
import { computed, reactive, ref, watchEffect } from "vue";
import Widget from "./components/Widget.vue";
import LineChart from "./components/LineChart.vue";
import BarChart from "./components/BarChart.vue";
import PieChart from "./components/PieChart.vue";
import FunnelChart from "./components/FunnelChart.vue";

// 1280*720、1920*1080
// 画布实际宽高
const canvasWidth = ref(1920);
const canvasHeight = ref(1080);
// 960/540、640/360
const widgetWidth = ref(960);
const widgetHeight = ref(540);

const canvasLeft = ref(0);
const canvasTop = ref(0);



// 4.整体等比例缩放
const canvasStyle = reactive({
  transform: "scale(1, 1)",
  transformOrigin: `left top`,
});
const equalScale = () => {
  // 当前窗口宽高比例
  let windowWidth = window.innerWidth;
  let windowHeight = window.innerHeight;
  let windowRatio = windowWidth / windowHeight;
  // 画布原始宽高比例
  const canvasRatio = canvasWidth.value / canvasHeight.value;
  // 计算画布适应后的新宽高
  let newCanvasWidth = 0;
  let newCanvasHeight = 0;
  // windowWidth / windowHeight = canvasWidth / canvasHeight
  if (canvasRatio > windowRatio) {
    // 画布的宽高比大于屏幕的宽高比
    // 画布的宽度调整为屏幕的宽度
    newCanvasWidth = windowWidth;
    // 画布的高度根据画布原比例进行缩放
    newCanvasHeight = windowWidth / canvasRatio;
  } else {
    // 画布的宽高比小于屏幕的宽高比
    // 画布的高度调整为屏幕的高度
    newCanvasHeight = windowHeight;
    // 画布的宽度根据画布原比例进行缩放
    newCanvasWidth = windowHeight * canvasRatio;
  }
  // 相对于画布原始宽高的缩放比例
  const scaleX = newCanvasWidth / canvasWidth.value;
  const scaleY = newCanvasHeight / canvasHeight.value;
  // 居中
  const translateX = (windowWidth - newCanvasWidth) / 2 / scaleX;
  const translateY = (windowHeight - newCanvasHeight) / 2 / scaleY;
  canvasStyle.transform = `scale(${scaleX}, ${scaleY}) translate(${translateX}px, ${translateY}px)`;
};

// 复位
const reset = () => {
  canvasWidth.value = 1920;
  canvasHeight.value = 1080;
  canvasLeft.value = 0;
  canvasTop.value = 0;
  canvasStyle.transform = "scale(1, 1)";
  canvasStyle.transformOrigin = `left top`;
};

const fit = () => {
  reset();
  equalScale();
};
watchEffect(() => {
  fit();
});
window.addEventListener("resize", () => {
  fit();
});
</script>

<template>
  <div class="canvasBox" >
    <div
      class="canvas"
      :style="{
        width: canvasWidth + 'px',
        height: canvasHeight + 'px',
        left: canvasLeft + 'px',
        top: canvasTop + 'px',
        ...canvasStyle,
      }"
    >
      <Widget
        :width="widgetWidth"
        :height="widgetHeight"
        :left="0"
        :top="0"
      >
        <LineChart></LineChart>
      </Widget>
      <Widget
        :width="widgetWidth"
        :height="widgetHeight"
        :left="widgetWidth"
        :top="0"
      >
        <BarChart></BarChart>
      </Widget>
      <Widget
        :width="widgetWidth"
        :height="widgetHeight"
        :left="0"
        :top="widgetHeight"
      >
        <PieChart></PieChart>
      </Widget>
      <Widget
        :width="widgetWidth"
        :height="widgetHeight"
        :left="widgetWidth"
        :top="widgetHeight"
      >
        <FunnelChart></FunnelChart>
      </Widget>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
<style scoped>
.canvasBox {
  width: 100vw;
  height: 100vh;
  overflow: "hidden";
}
.canvas {
  position: relative;
}
</style>
