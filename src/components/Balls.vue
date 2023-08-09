<template>
  <div>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
export default {
  mounted() {
    this.ballCount = 100;
    this.x = [];
    this.y = [];
    this.xSpeed = [];
    this.ySpeed = [];
    this.r = [];
    this.g = [];
    this.b = [];
    this.size = [];

    this.setup();

    this.interval = setInterval(this.draw, 16);
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  methods: {
    setup() {
      const canvas = this.$refs.canvas;
      this.ctx = canvas.getContext("2d");
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      for (let i = 0; i < this.ballCount; i++) {
        this.x[i] = canvas.width / 2;
        this.y[i] = canvas.height / 2;
        this.xSpeed[i] = this.random(-5, 5);
        this.ySpeed[i] = this.random(-5, 5);
        this.size[i] = this.random(10, 50);
        this.r[i] = this.random(0, 256);
        this.g[i] = this.random(0, 256);
        this.b[i] = this.random(0, 256);
      }
    },
    draw() {
      const canvas = this.$refs.canvas;
      this.ctx.clearRect(0, 0, canvas.width, canvas.height);
      this.ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
      this.ctx.fillRect(0, 0, canvas.width, canvas.height);

      for (let i = 0; i < this.ballCount; i++) {
        this.x[i] += this.xSpeed[i];
        this.y[i] += this.ySpeed[i];
        if (this.x[i] < 0 || this.x[i] > canvas.width) {
          this.xSpeed[i] *= -1;
        }
        if (this.y[i] < 0 || this.y[i] > canvas.height) {
          this.ySpeed[i] *= -1;
        }
        this.ctx.fillStyle = `rgb(${this.r[i]}, ${this.g[i]}, ${this.b[i]})`;
        this.ctx.beginPath();
        this.ctx.arc(this.x[i], this.y[i], this.size[i], 0, Math.PI * 2);
        this.ctx.fill();
      }
    },
    random(min, max) {
      return Math.random() * (max - min) + min;
    },
  },
};
</script>

<style scoped>
canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>