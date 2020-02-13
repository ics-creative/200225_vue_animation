<template>
  <svg xmlns="http://www.w3.org/2000/svg" width="600" height="400" viewBox="0 0 600 400" >
    <path d="M0,200 L600,200" stroke="#aaaaaa" stroke-width="1" fill="none" />
    <path :d="pathStr" stroke="#888888" stroke-width="2" fill="none" />
  </svg>
</template>

<script>

const POINTS_COUNT = 50
const MAX_Y = 100
const WIDTH = 600
const HEIGHT = 400
const EASE = 0.4
const SPEED = 0.002
const WAVE_SCALE = (1 / Math.PI) * 0.5

const valuesToPathStr = (values) => {
  if (!values.length) { return 'M0,0' }
  const points = values.map((y, x) => ({ x: x / (POINTS_COUNT - 1) * WIDTH, y: y * MAX_Y + HEIGHT / 2 }))
  const firstPoint = points.shift()
  const controlX = WIDTH / (POINTS_COUNT - 1) * EASE
  return `M${firstPoint.x},${firstPoint.y} S` +
    points.map(p => `${p.x - controlX},${p.y} ${p.x},${p.y}`).join(' ')
}

export default {
  data () {
    return {
      time: 0
    }
  },
  computed: {
    values () {
      return new Array(POINTS_COUNT).fill(0).map((_, index) => {
        const x = index / POINTS_COUNT // x座標(0-1)
        const yBase = index % 2 === 0 ? 1 : -1 // y座標(-1と1を交互に設定)
        const y = Math.sin(x / WAVE_SCALE - this.time * SPEED) * yBase // sin波を掛ける
        return y
      })
    },
    pathStr () {
      return valuesToPathStr(this.values)
    }
  },
  mounted () {
    const startTime = Date.now()
    const update = () => {
      this.time = Date.now() - startTime
      requestAnimationFrame(update)
    }
    update()
  }
}
</script>
