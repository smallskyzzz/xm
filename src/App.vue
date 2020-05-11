<template>
  <div id="app" ref="app">
    <canvas class="canvas" ref="canvas"></canvas>
    <p>距离那一刻相差<br>{{ time }}</p>
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  data () {
    return {
      time: ''
    }
  },
  mounted () {
    const canvas = this.$refs.canvas
    canvas.width = this.$refs.app.clientWidth
    canvas.height = this.$refs.app.clientHeight
    const ctx = canvas.getContext('2d')
    let ballsArr = []
    function Ball (x, y) {
      this.x = x
      this.y = y
      this.r = 40
      this.color = 'rgba(' + parseInt(Math.random() * 256) + ',' + parseInt(Math.random() * 256) + ',' + parseInt(Math.random() * 256) + ', 0.8)'
      this.dx = parseInt(Math.random() * 18) - 9
      this.dy = parseInt(Math.random() * 18) - 9
      ballsArr.push(this)
    }

    Ball.prototype.update = function () {
      this.x += this.dx
      this.y += this.dy
      this.r--
      // eslint-disable-next-line no-unused-expressions
      this.r < 0 ? this.godie() : ''
    }

    Ball.prototype.render = function () {
      ctx.beginPath()
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2, true)
      ctx.closePath()
      ctx.fillStyle = this.color
      ctx.fill()
    }

    Ball.prototype.godie = function () {
      var _this = this
      ballsArr.forEach(function (value, index) {
        // eslint-disable-next-line no-unused-expressions
        value === _this ? ballsArr.splice(index, 1) : ''
      })
    }

    console.log(canvas)
    setTimeout(function () {
      canvas.ontouchmove = function (event) {
        for (let i = 0; i < 10; i++) {
          // eslint-disable-next-line no-new
          new Ball(event.touches[0].pageX, event.touches[0].pageY)
        }
      }
    })
    setInterval(function () {
      ctx.clearRect(0, 0, canvas.width, canvas.height)
      for (var i = 0; i < ballsArr.length; i++) {
        ballsArr[i].update()
        ballsArr[i] && ballsArr[i].render()
      }
    }, 20)

    var _this = this
    setInterval(function () {
      _this.timeFn('2019-10-26 18:40:00')
    })
  },
  methods: {
    timeFn: function (d1) {
      var dateBegin = new Date(d1.replace(/-/g, '/'))
      var dateEnd = new Date()
      var dateDiff = dateEnd.getTime() - dateBegin.getTime()
      var dayDiff = Math.floor(dateDiff / (24 * 3600 * 1000))
      var leave1 = dateDiff % (24 * 3600 * 1000)
      var hours = Math.floor(leave1 / (3600 * 1000))
      // 计算相差分钟数
      var leave2 = leave1 % (3600 * 1000)
      var minutes = Math.floor(leave2 / (60 * 1000))
      // 计算相差秒数
      var leave3 = leave2 % (60 * 1000)
      var seconds = Math.round(leave3 / 1000)
      this.time = ' 相差 ' + dayDiff + ' 天 ' + hours + '小时 ' + minutes + ' 分钟 ' + seconds + ' 秒'
      // console.log(' 相差 ' + dayDiff + '天 ' + hours + '小时 ' + minutes + ' 分钟' + seconds + ' 秒')
      // console.log(dateDiff + '时间差的毫秒数', dayDiff + '计算出相差天数', leave1 + '计算天数后剩余的毫秒数'
      //   , hours + '计算出小时数', minutes + '计算相差分钟数', seconds + '计算相差秒数')
    }
  },
  components: {
    HelloWorld
  }
}
</script>

<style lang="scss" scoped>
  #app{
    .canvas{
      position: relative;
      width: 100%;
      height: 100%;
      background-color: #333;
    }
  }
</style>
