<template>
  <div class="box-cat"
    :style="{
      transform: `translate(${x}px, 250px)`
    }"
  />
</template>

<script>
export default {
  data () {
    return {
      x: 100
    }
  },
  methods: {
    moveTo (x) {
      this.x = x
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
  background: url(/img/BoxCat.svg) no-repeat center;
  transform-origin: 50% 100%;
  transition: transform 500ms;
}
</style>
