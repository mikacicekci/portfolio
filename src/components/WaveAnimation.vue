<template>
  <div style="height: 100%; overflow: none">
    <canvas ref="canvas" />
  </div>
</template>

<script>
export default {
  name: "WaveAnimation",
  data() {
    return {
      squareSize: 50,
      littleSquareSize: 6,
      angleIncrement: 26,
      animationIncrement: 1,
      animationSpeedMs: 10,
      color: "white",
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.init();
      this.animate();
    });
  },
  unmounted() {
    clearInterval(this.interval);
  },
  methods: {
    init() {
      let canvas = this.$refs["canvas"];

      let containerWidth = this.$refs["canvas"].parentElement.offsetWidth;
      let containerHeight = this.$refs["canvas"].parentElement.offsetHeight;

      this.squaresCnt = {
        lign: Math.trunc(containerWidth / this.squareSize),
        col: Math.trunc(containerHeight / this.squareSize),
      };

      canvas.width = this.squareSize * this.squaresCnt.lign;
      canvas.height = this.squareSize * this.squaresCnt.col;

      canvas.style.margin =
        Math.trunc((containerHeight - canvas.height) / 2) +
        "px " +
        Math.trunc((containerWidth - canvas.width) / 2) +
        "px";
    },
    draw(angleOffset) {
      let ctx = this.$refs["canvas"].getContext("2d");
      ctx.clearRect(
        0,
        0,
        this.$refs["canvas"].width,
        this.$refs["canvas"].height
      );
      ctx.fillStyle = this.color;

      for (let i = 0; i < this.squaresCnt.lign; i++) {
        for (let j = 0; j < this.squaresCnt.col; j++) {
          let [xPos, yPos] = this.getPos(
            (angleOffset + (i + j) * this.angleIncrement) % 360,
            Math.trunc(this.squareSize / 2 - this.littleSquareSize - 1)
          );

          ctx.save();
          ctx.fillStyle = this.color;
          ctx.translate(
            xPos + (i + 1 / 2) * this.squareSize,
            yPos + (j + 1 / 2) * this.squareSize
          );
          ctx.rotate(
            -this.toRad((angleOffset + (i + j) * this.angleIncrement) % 360)
          );
          ctx.fillRect(0, 0, this.littleSquareSize, this.littleSquareSize);
          ctx.restore();
        }
      }
    },
    animate() {
      let angleOffset = 0;
      this.interval = setInterval(() => {
        this.draw((angleOffset += this.animationIncrement));
      }, this.animationSpeedMs);
    },
    stopAnimation() {
      clearInterval(this.interval);
    },
    toRad(degrees) {
      return (Math.PI / 180) * degrees;
    },
    getPos(angle, radius) {
      return [
        Math.round(Math.cos(this.toRad(angle)) * radius),
        Math.round(Math.sin(this.toRad(angle)) * radius),
      ];
    },
  },
};
</script>

<style>
</style>