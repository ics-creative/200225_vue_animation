<template>
  <AnimBox
    ref="cat"
    class="box-cat3"
    img="img/BoxCat.svg"
    :w="size" :h="size"
    :x="100" :y="groundY"
    :ox="50" :oy="100"
  />
</template>

<script>
import AnimBox from './AnimBox'

export default {
  components: {
    AnimBox
  },
  data () {
    return {
      groundY: 250, // 地面の高さ
      size: 80, // 猫のサイズ
      lastX: 100 // 直近のx座標
    }
  },
  methods: {
    async moveTo (x, y) {
      const cat = this.$refs.cat
      const jumpHeight = this.groundY - y // ジャンプの高さ
      const isRightMove = x > this.lastX // 移動方向（右: true, 左: false）
      const rotationDir = isRightMove ? 1 : -1 // 回転の向き（移動方向によって「溜め」の回転方向が異なる）
      this.lastX = x
      await cat.animTo({ r: -10 * rotationDir }, 200) // 横移動の「溜め」
      await cat.animTo({ x: x - this.size, r: 20 * rotationDir }, 500) // 横移動+回転
      await cat.animTo({ r: 0 }, 100) // 回転を戻す
      await cat.animTo({ y: 20, sy: 0.8 }, 300) // ジャンプの「溜め」
      await cat.animTo({ y: -jumpHeight, sy: 1.2 }, 200) // ジャンプ往き+伸び
      await cat.animTo({ y: 0, sy: 0.8 }, 200) // ジャンプ戻り+縮み
      await cat.animTo({ sy: 1.0 }, 100) // 縮みを戻す
    }
  }
}
</script>

<style scoped>
.box-cat3 {
  margin-left: -40px;
}
</style>
