<template>
  <div id="snakeContainer" @click="clickHandler">
    <canvas ref="snakeCanvas" id="snakeCanvas" />
    <canvas
      ref="scoreCanvas"
      id="scoreCanvas"
      :style="{ 'font-size': scoreHeight + 'px' }"
    />
    <p id="snakePlayMessage" ref="snakePlayMessage" v-show="!isPlaying">
      Hit SPACE to Start!
    </p>
  </div>
</template>

<script>
function Point(x, y) {
  this.x = x;
  this.y = y;

  // gives next position according to current direction in dirTab.
  // Will be length-1 for head and 0 for tail (cleaner)
  this.incPos = function (point) {
    this.x += point.x;
    this.y += point.y;
  };
}

export default {
  name: "Snake",
  data() {
    return {
      /*
       * CANVAS VARIABLES
       */
      snakeContainerPercentage: 0.8,
      dimension: 0,
      nbBlocks: 20,
      blockSize: 0,
      colors: {
        snake: "navy",
        block: "white",
        food: "#00FF00",
        background: "orange",
        score: "red",
      },
      /*
       * SNAKE VARIABLES
       */
      dirArr: undefined,
      cptArr: undefined,
      newDir: undefined,
      blockMatrix: undefined,
      snakeHead: undefined,
      cleaner: undefined,
      /*
       *	GAME VARIABLES
       */
      scoreInc: 100,
      scoreHeight: 41,
      score: 0,
      isPlaying: false,
      foodTimeoutMs: 1000,
      intervalTimeMs: 100,
      interval: undefined,
      foodTimeout: undefined,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.prepareGame();
    });
  },
  unmounted() {
    this.foodTimeout && clearTimeout(this.foodTimeout);
    this.interval && clearInterval(this.interval);
  },
  methods: {
    prepareGame() {
      this.initCanvas();
      this.initVars();
      this.initMatrix();
      this.dispScore();
      this.drawSnake();

      document.addEventListener("keydown", this.starterKeydownHandler);
    },
    startGame() {
      document.removeEventListener("keydown", this.starterKeydownHandler);
      document.addEventListener("keydown", this.keydownHandler);

      this.isPlaying = true;
      this.moveSnake();
      this.interval = setInterval(this.moveSnake, this.intervalTimeMs);
      this.addFood();
    },
    endGame() {
      document.removeEventListener("keydown", this.keydownHandler);
      this.isPlaying = false;
      this.foodTimeout && clearTimeout(this.foodTimeout);
      this.prepareGame();
      this.interval && clearInterval(this.interval);
    },
    initCanvas() {
      let snakeCanvas = this.$refs["snakeCanvas"];
      let scoreCanvas = this.$refs["scoreCanvas"];

      let containerWidth = this.$refs.snakeCanvas.parentElement.offsetWidth;
      let containerHeight = this.$refs.snakeCanvas.parentElement.offsetHeight;

      let minDim =
        Math.min(containerWidth, containerHeight) *
        this.snakeContainerPercentage;
      this.dimension = Math.floor(minDim / this.nbBlocks) * this.nbBlocks;

      snakeCanvas.width = snakeCanvas.height = this.dimension;
      scoreCanvas.width = this.dimension;
      scoreCanvas.height = this.scoreHeight;

      let canvasMarginProp = (containerHeight - this.dimension + 2) / 2 + "px ";

      scoreCanvas.style.top = snakeCanvas.style.top = canvasMarginProp;
      scoreCanvas.style.left = snakeCanvas.style.left =
        (containerWidth - this.dimension + 2) / 2 + "px";
      this.$refs.snakePlayMessage.style.bottom = canvasMarginProp;

      this.blockSize = this.dimension / this.nbBlocks;
      var ctx = scoreCanvas.getContext("2d");
      ctx.fillStyle = this.colors["block"];
      ctx.fillRect(0, 0, this.dimension, this.dimension);
    },
    initVars() {
      // snake's first position in the middle of our container
      this.dirArr = [new Point(1, 0)];
      this.cptArr = [1];
      this.blockMatrix = [];
      this.newDir = 0;
      this.score = 0;

      this.snakeHead = new Point(
        parseInt(this.nbBlocks / 2) - 1,
        parseInt(this.nbBlocks / 2)
      );
      // Variable cleaner must be right behind the snake's tail
      this.cleaner = new Point(
        this.snakeHead.x - this.dirArr[0].x * (this.cptArr[0] + 1),
        this.snakeHead.y - this.dirArr[0].y * this.cptArr[0]
      );
    },
    initMatrix() {
      for (let i = 0; i < this.nbBlocks; i++) {
        let col = [];
        for (let j = 0; j < this.nbBlocks; j++) {
          col.push("block");
        }
        this.blockMatrix.push(col);
      }
    },
    drawSnake() {
      let tail = new Point(
        this.snakeHead.x - this.dirArr[0].x,
        this.snakeHead.y - this.dirArr[0].y
      );
      this.updateBlock(tail, "snake");
      this.updateBlock(this.snakeHead, "snake");
    },
    dispScore() {
      let ctx = this.$refs["scoreCanvas"].getContext("2d");
      ctx.font = this.scoreHeight + "px Impact";
      ctx.lineWidth = 1;
      ctx.fillStyle = this.colors["score"];
      ctx.clearRect(0, 0, this.dimension, this.scoreHeight);
      ctx.fillText(this.score, 2, 41);
    },
    updateBlock(point, type, size = this.blockSize) {
      this.blockMatrix[point.x][point.y] = type;
      this.colorBlock(point, this.colors[type], size);
    },
    colorBlock(point, color, size) {
      let ctx = this.$refs["snakeCanvas"].getContext("2d");
      ctx.lineWidth = 1;
      ctx.fillStyle = color;

      let dim = size ? size : this.blockSize;
      ctx.fillRect(
        point.x * this.blockSize + (this.blockSize - dim) / 2,
        point.y * this.blockSize + (this.blockSize - dim) / 2,
        dim,
        dim
      );
    },
    collision() {
      return (
        this.snakeHead.x > this.nbBlocks - 1 ||
        this.snakeHead.x < 0 ||
        this.snakeHead.y > this.nbBlocks - 1 ||
        this.snakeHead.y < 0 ||
        this.getBlockType(this.snakeHead) == "snake"
      );
    },
    // Add the blocks to the container and fill them with blockColor
    getBlockType(point) {
      return this.blockMatrix[point.x][point.y];
    },
    moveSnake() {
      this.dirLock = false;
      this.snakeHead.incPos(this.dirArr[this.dirArr.length - 1]);

      if (this.collision()) {
        this.endGame();
        return;
      } else if (this.getBlockType(this.snakeHead) != "food") {
        this.cleaner.incPos(this.dirArr[0]);
        if (this.cptArr[0] == 0) {
          this.cptArr.shift();
          this.dirArr.shift();
        }
        this.cptArr[0]--;
      } else {
        this.updScore();
        this.addFood();
      }

      this.cptArr[this.cptArr.length - 1]++;
      this.updateBlock(this.snakeHead, "snake");
      this.updateBlock(this.cleaner, "block");
    },
    keydownHandler(e) {
      if (this.dirLock) {
        return;
      }
      let dir;
      if (e.key == "ArrowLeft") dir = new Point(-1, 0);
      else if (e.key == "ArrowUp") dir = new Point(0, -1);
      else if (e.key == "ArrowRight") dir = new Point(1, 0);
      else if (e.key == "ArrowDown") dir = new Point(0, 1);
      else return;
      if (Math.abs(dir.x) != Math.abs(this.dirArr[this.dirArr.length - 1].x)) {
        this.dirArr.push(dir);
        this.cptArr.push(0);
        this.dirLock = true;
      }
    },
    starterKeydownHandler(e) {
      if (e.code == "Space") {
        this.startGame();
      }
    },
    updScore() {
      this.incScore();
      this.dispScore();
    },
    getRandPos() {
      return Math.floor(Math.random() * (this.nbBlocks - 1));
    },
    incScore() {
      this.score += this.scoreInc;
    },
    addFood() {
      let time = Math.floor(Math.random() * this.foodTimeoutMs) + 1;

      let that = this;

      this.foodTimeout = setTimeout(function () {
        let foodPnt = new Point();

        do {
          foodPnt = new Point(that.getRandPos(), that.getRandPos());
        } while (that.getBlockType(foodPnt) != "block");

        that.updateBlock(foodPnt, "food", that.blockSize / 2);
      }, time);
    },
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

#scoreCanvas {
  z-index: 2;
  position: absolute;
  top: 0;
  left: 0;
}

#snakeContainer {
  position: relative;
  width: 100%;
  height: 100%;
}

#snakeCanvas {
  position: absolute;
  border: 1px solid black;
  background-color: white;
  z-index: 1;
}

#snakePlayMessage {
  font-size: 30px;
  width: 100%;
  text-align: center;
  color: red;
  position: absolute;
  z-index: 2;
  bottom: 0;
  font-family: "Press Start 2P", cursive;
  animation: blink 1s steps(4, start) infinite;
}

@keyframes blink {
  to {
    visibility: hidden;
  }
}
</style>
