<template>
  <div class="chara-box"
    :style="{
      backgroundImage: img ? `url(${img})` : 'none',
      width: `${w}px`,
      height: `${h}px`,
      transform: `translate(${x}px, ${y}px) rotate(${r}deg) scale(${sx}, ${sy})`,
      transformOrigin: `${ox}% ${oy}%`,
      transition: `transform ${dur}ms`
    }"
    @transitionstart="startAnim"
    @transitionend="endAnim"
  >
    <slot />
  </div>
</template>

<script>
export default {
  name: 'CharaBox',
  props: {
    img: { type: String, default: '' },
    x: { type: Number, default: 0 },
    y: { type: Number, default: 0 },
    w: { type: Number, default: 100 },
    h: { type: Number, default: 100 },
    r: { type: Number, default: 0 },
    sx: { type: Number, default: 1 },
    sy: { type: Number, default: 1 },
    ox: { type: Number, default: 50 },
    oy: { type: Number, default: 50 },
    dur: { type: Number, default: 0 }
  },
  data () {
    return {
    }
  },
  methods: {
    startAnim (ev) {
      console.log('start', ev)
      this.$emit('start', ev)
    },
    endAnim (ev) {
      console.log('end', ev)
      this.$emit('finish', ev)
    }
  }
}
</script>

<style lang="scss" scoped>
  .chara-box {
    position: absolute;
    border: 0.5px solid gray;
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
  }
</style>
