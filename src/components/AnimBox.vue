<template>
  <div
    class="anim-box"
    :style="{
      backgroundImage: img ? `url(${img})` : 'none',
      width: `${w}px`,
      height: `${h}px`,
      transform: `
        translate(${transforms.x}px,${transforms.y}px)
        rotate(${transforms.r}deg)
        scale(${transforms.sx}, ${transforms.sy})`,
      transformOrigin: `${ox}% ${oy}%`,
      transition: `transform ${duration}ms ${delay}ms ${easing}`
    }"
  >
    <slot />
  </div>
</template>

<script>
export default {
  name: 'AnimBox',
  props: {
    img: { type: String, default: null },
    x: { type: Number, default: 0 },
    y: { type: Number, default: 0 },
    w: { type: Number, default: 100 },
    h: { type: Number, default: 100 },
    r: { type: Number, default: 0 },
    sx: { type: Number, default: 1 },
    sy: { type: Number, default: 1 },
    ox: { type: Number, default: 50 },
    oy: { type: Number, default: 50 },
    delay: { type: Number, default: 0 }
  },
  data () {
    return {
      relatives: {},
      duration: 1000,
      easing: 'ease'
    }
  },
  computed: {
    /**
     * propsで指定されたペースの値と、アニメーション用の差分値を合成してtransform用の値を返す
     */
    transforms () {
      return {
        x: this.x + (this.relatives.x || 0),
        y: this.y + (this.relatives.y || 0),
        r: this.r + (this.relatives.r || 0),
        sx: this.sx * (this.relatives.sx || 1),
        sy: this.sy * (this.relatives.sy || 1)
      }
    }
  },
  methods: {
    async animTo (relatives, dur = 1000, easing = 'ease') {
      // 指定されたプロパティのみ上書き。指定されなかったものは前回値のままにする
      const current = this.$data.relatives
      this.$data.relatives = {
        x: relatives.x ?? current.x,
        y: relatives.y ?? current.y,
        r: relatives.r ?? current.r,
        sx: relatives.sx ?? current.sx,
        sy: relatives.sy ?? current.sy
      }
      this.$data.duration = dur
      this.$data.easing = easing
      return new Promise(resolve => window.setTimeout(resolve, dur))
    }
  }
}
</script>

<style scoped>
.anim-box {
  position: absolute;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
}
</style>
