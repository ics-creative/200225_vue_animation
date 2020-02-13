<template>
  <div class="">
    <button @click="jump">Jump</button>
    <div class="stage" @click="stageClicked">
      <AnimBox ref="tama"  :y="100">
        <AnimBox ref="jumpBox" img="/img/tama.svg" :oy="100" />
      </AnimBox>
    </div>
  </div>
</template>

<script>
import AnimBox from './AnimBox'
export default {
  name: 'MyCharactor',
  components: { AnimBox },
  props: {
  },
  data () {
    return {
      lastX: 0
    }
  },
  methods: {
    async jump () {
      const tama = this.$refs.jumpBox
      await tama.animTo({ sy: 0.8 }, 200)
      await tama.animTo({ sy: 1.1, y: -100 }, 500, 'ease-out')
      await tama.animTo({ sy: 0.8, y: 0 }, 500, 'ease-in')
      await tama.animTo({ sy: 1.0 }, 300)
    },
    async runTo (x) {
      const tama = this.$refs.tama
      if (x > this.lastX) {
        await tama.animTo({ sx: 1 }, 0)
        await tama.animTo({ r: -5 }, 300)
        await tama.animTo({ x: x, r: 15 }, 3000)
        await tama.animTo({ r: -5 }, 300)
        await tama.animTo({ r: 0 }, 800)
      } else {
        await tama.animTo({ sx: -1 }, 0) // 反転
        await tama.animTo({ r: 5 }, 300)
        await tama.animTo({ x: x, r: -15 }, 3000)
        await tama.animTo({ r: 5 }, 300)
        await tama.animTo({ r: 0 }, 800)
      }
      this.lastX = x
    },
    stageClicked (ev) {
      this.runTo(ev.offsetX)
    }
  }
}
</script>

<style lang="scss" scoped>
.stage {
  width: 100%;
  height: 400px;
}
</style>
