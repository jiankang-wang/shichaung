<template>
  <div class="hello">
    <div class="imgs">
      <img src="../assets/timg.jpeg" />
      <canvas ref="canvas" class="canvas">
      </canvas>
    </div>
    <div class="operity">
      <div @click="reset" class="reset">RESET</div>
      <div @click="show" class="show">SHOW</div>
    </div>
  </div>
</template>

<script>
import imgsrc from '../assets/timg.jpeg'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      canvasWidth: 800,
      canvasHeight: 600,
      context: '',
      imgsrc: imgsrc,
      clipingRegion: {
        x: 450,
        y: 170,
        r: 50
      }
    }
  },
  methods: {
    reset() {
      this.clipingRegion.r = 50
      this.drawcanvas()
    },
    show() {
      this.clipingRegion.r = 1000
      this.drawcanvas()
    },
    drawcanvas() {
      let canvas = this.$refs.canvas
      this.clipingRegion.x = Math.random() * (canvas.width - 1 * this.clipingRegion.r)
      this.clipingRegion.y = Math.random() * (canvas.height - 1 * this.clipingRegion.r)
      // 获取上下文
      this.context = canvas.getContext('2d')
      canvas.width = this.canvasWidth
      canvas.height = this.canvasHeight
      // 绘制图像
      var img = new Image()
      img.src = this.imgsrc
      setTimeout(() => {
        this.context.clearRect(0, 0, canvas.width, canvas.height)
        this.context.save() // 保存
        // 绘制视窗
        this.context.beginPath()
        // 绘制圆形（x坐标， y坐标， 半径， 起始位置， 圆形， 事是否是逆时针）
        this.context.arc(this.clipingRegion.x, this.clipingRegion.y, this.clipingRegion.r, 0, Math.PI * 2, false)
        this.context.clip() // 进行裁剪
        // 绘制图片
        this.context.drawImage(img, 0, 0)
        this.context.restore() // 状态恢复
      }, 10)
    },
  },
  mounted() {
   this.drawcanvas();
  }
}
</script>

<style scoped>
.hello {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
h1, h2 {
  font-weight: normal;
}
.imgs {
  position: relative;
  width: 800px;
  height: 600px;
}
img {
  position: absolute;
  top: 0;
  left: 0;
  display: block;
  width: 800px;
  height: 600px;
  filter: blur(20px);
  -webkit-filter: blur(20px);
  z-index: 0;
}
.canvas {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  margin: 0 auto;
  width: 800px;
  height: 840px;
  z-index: 100;
}
.operity {
  margin-top: 20px;
  display: flex;
}
.reset,.show {
  padding: 20px;
}
.reset {
  margin-right: 1000px;
  background-color: green;
  color: #fff;
}
.show {
  background-color: blue;
  color: #fff;
}
</style>
