<template>
  <div id="Application">
    <div class="container">
      <!-- 挡板 -->
      <div class="board" :style="{ left: boardX + 'px' }"></div>
      <!-- 球 -->
      <div class="ball" :style="{ left: ballX + 'px', top: ballY + 'px' }"></div>
      <!-- <h1>游戏结束提示</h1> -->
      <h1 v-if="fail" style="text-align: center">游戏失败</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyApp',
  data() {
    return {
      // 控制挡板位置
      boardX: 0,
      // 控制弹珠位置
      ballX: 0,
      ballY: 0,
      // 控制弹珠移动速度
      rateX: 0.1,
      rateY: 0.1,
      // 提示
      fail: false
    }
  },
  mounted() {
    this.enterKeyup()
    this.rateX = Math.random() + 0.1
    this.rateY = Math.random() + 0.1
    console.log(this.rateX)
    this.timer = setInterval(() => {
      // 到达右侧边缘进行反弹
      if (this.ballX + this.rateX >= 440 - 30) {
        // this.rateX *= -1
        this.rateX = this.rateX * -1
      }
      // 到达左侧边缘进行反弹
      if (this.ballX + this.rateX <= 0) {
        this.rateX *= -1
      }

      // 到达上侧边缘进行反弹
      if (this.ballY + this.rateY <= 0) {
        this.rateY *= -1
      }

      this.ballX += this.rateX
      this.ballY += this.rateY
      // 失败判定
      if (this.ballY >= 440 - 30 - 10) {
        if (this.boardX <= this.ballX + 30 && this.boardX + 80 >= this.ballX) {
          this.rateY *= -1
        } else {
          clearInterval(this.timer)
          this.fail = true
        }
      }
    }, 2)
  },
  methods: {
    keydown(e) {
      if (e.key == 'ArrowLeft') {
        if (this.boardX > 10) {
          this.boardX -= 20
        }
      } else if (e.key == 'ArrowRight') {
        if (this.boardX < 440 - 80) {
          this.boardX += 20
        }
      }
    },
    enterKeyup() {
      document.addEventListener('keydown', this.keydown)
    }
  }
}
</script>

<style scoped>
.container {
  position: relative;
  margin: 0 auto;
  width: 440px;
  height: 440px;
  background-color: blanchedalmond;
}
.ball {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 30px;
  height: 30px;
  background-color: orange;
  border-radius: 30px;
}

.board {
  position: absolute;
  left: 0;
  bottom: 0;
  height: 10px;
  width: 80px;
  border-radius: 5px;
  background-color: red;
}
</style>
