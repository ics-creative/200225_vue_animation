<template>
  <div class="box-cat"
    :style="{
      transform: `translate(${x}px, ${y}px)`,
      transition: `transform ${dur}ms`
    }"
  />
</template>

<script>
/** 指定のミリ秒待つPromiseを返す */
const wait = (ms) => new Promise(resolve => setTimeout(resolve, ms))

export default {
  data () {
    return {
      x: 100,
      y: 250,
      dur: 500
    }
  },
  methods: {
    async moveTo (x, y) {
      const initialY = this.y // 移動前のyを覚えておく
      this.x = x // 指定された座標のxまで移動
      this.dur = 500
      await wait(this.dur)
      this.y = y // 指定された座標のyまでジャンプ
      this.dur = 200
      await wait(this.dur)
      this.y = initialY // 最初のy座標に戻る
      this.dur = 200
      await wait(this.dur)
    }
  }
}
</script>

<style scoped>
.box-cat {
  position: absolute;
  width: 80px;
  height: 80px;
  margin-left: -40px;
  box-sizing: border-box;
  background: url(../../public/img/BoxCat.svg) no-repeat center;
  transform-origin: 50% 100%;
}
</style>
