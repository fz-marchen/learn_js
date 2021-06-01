<template>
  <div class="canvas">
    <div class="canvas-left"></div>

    <div class="canvas-md">
      <!-- <canvas id="canvas"></canvas> -->
      <canvas id="canvas" :width="width" :height="height"></canvas>
    </div>

    <!-- {{ width }}
    {{ height }} -->
    <div class="canvas-right">
      <div @click="drawRect">长方形</div>
      <div @click="drawSjx">三角形</div>
      <div @click="drawYx">圆形</div>
      <div @click="drawQc">贝塞尔曲线</div>
      <div @click="drawColor1">填充颜色</div>
      <div @click="drawColor2">非填充颜色</div>
      <div @click="drawYl">圆脸</div>

      <div @click="save">save</div>
      <div @click="restore">restore</div>
    </div>
  </div>
</template>

<script setup>
// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md
import {
  defineComponent,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered,
  reactive,
  watch,
  ref,
} from "vue";

let width = ref(0);
let height = ref(0);

let canvas = null;
let ctx = null;

onMounted(() => {
  if (true) {
    width.value = 500;
    height.value = 500;
  }
  setTimeout(() => {
    canvas = document.getElementById("canvas");
    ctx = canvas.getContext("2d");
  }, 300);
});

function drawRect() {
  // ctx.fillStyle = "rgb(200,0,0)";
  // ctx.fillRect(10, 10, 55, 50);

  // ctx.fillStyle = "rgba(0, 0, 200, 0.5)";
  // ctx.fillRect(30, 30, 55, 50);

  ctx.fillRect(25, 25, 100, 100);
  ctx.clearRect(45, 45, 60, 60);
  ctx.strokeRect(50, 50, 50, 50);
  save();

  restore();
}

function drawSjx() {
  // 填充三角形
  ctx.beginPath();
  ctx.moveTo(25, 25);
  ctx.lineTo(105, 25);
  ctx.lineTo(25, 105);
  ctx.fill();

  // 描边三角形
  ctx.beginPath();
  ctx.moveTo(125, 125);
  ctx.lineTo(125, 45);
  ctx.lineTo(45, 125);
  ctx.closePath();
  ctx.stroke();
}

function drawYx() {
  for (var i = 0; i < 4; i++) {
    for (var j = 0; j < 3; j++) {
      ctx.beginPath();
      var x = 50 + j * 100; // x 坐标值
      var y = 50 + i * 100; // y 坐标值
      var radius = 40; // 圆弧半径
      var startAngle = 0; // 开始点
      var endAngle = Math.PI + (Math.PI * j) / 2; // 结束点
      var anticlockwise = i % 2 == 0 ? false : true; // 顺时针或逆时针

      ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);

      if (i > 1) {
        ctx.fill();
      } else {
        ctx.stroke();
      }
    }
  }
}

function drawYl() {
  ctx.beginPath();
  ctx.arc(75, 75, 50, 0, Math.PI * 2, true); // 绘制
  ctx.moveTo(110, 75);
  ctx.arc(75, 75, 35, 0, Math.PI, false); // 口(顺时针)
  ctx.moveTo(65, 65);
  ctx.arc(60, 65, 5, 0, Math.PI * 2, true); // 左眼
  ctx.moveTo(95, 65);
  ctx.arc(90, 65, 5, 0, Math.PI * 2, true); // 右眼
  ctx.stroke();
}

function drawQc() {
  // 二次贝塞尔曲线
  ctx.beginPath();
  ctx.moveTo(75, 25);
  ctx.quadraticCurveTo(25, 25, 25, 62.5);
  ctx.quadraticCurveTo(25, 100, 50, 100);
  ctx.quadraticCurveTo(50, 120, 30, 125);
  ctx.quadraticCurveTo(60, 120, 65, 100);
  ctx.quadraticCurveTo(125, 100, 125, 62.5);
  ctx.quadraticCurveTo(125, 25, 75, 25);
  ctx.stroke();

  //三次贝塞尔曲线
  // ctx.beginPath();
  // ctx.moveTo(75, 40);
  // ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
  // ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
  // ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
  // ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
  // ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
  // ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
  // ctx.fill();
}

function drawColor1() {
  for (var i = 0; i < 6; i++) {
    for (var j = 0; j < 6; j++) {
      ctx.fillStyle =
        "rgb(" +
        Math.floor(255 - 42.5 * i) +
        "," +
        Math.floor(255 - 42.5 * j) +
        ",0)";
      ctx.fillRect(j * 25, i * 25, 25, 25);
    }
  }
}

function drawColor2() {
  for (var i = 0; i < 6; i++) {
    for (var j = 0; j < 6; j++) {
      ctx.strokeStyle =
        "rgb(0," +
        Math.floor(255 - 42.5 * i) +
        "," +
        Math.floor(255 - 42.5 * j) +
        ")";
      ctx.beginPath();
      ctx.arc(12.5 + j * 25, 12.5 + i * 25, 10, 0, Math.PI * 2, true);
      ctx.stroke();
    }
  }
}
function save() {
  console.log("save");
  ctx.save();
}
function restore() {
  console.log("restore");
  ctx.restore();
}
</script> 

<style lang="scss" scoped>
.canvas {
  display: flex;
  &-left {
    width: 200px;
    border-right: 1px solid red;
  }
  &-md {
    flex: 1;
  }
  &-right {
    width: 200px;
    border-left: 1px solid red;
  }
}
</style>