<template>
  <svg xmlns="http://www.w3.org/2000/svg" width="600" height="400" viewBox="0 0 600 400" >
    <path d="M0,200 L600,200" stroke="#aaaaaa" stroke-width="1" fill="none" />
    <path :d="pathStr" stroke="#888888" stroke-width="2" fill="none">
      <animate
        ref="anim"
        attributeName="d"
        repeatCount="1"
        dur="1.5s"
        fill="freeze"
        calcMode="spline"
        keyTimes="0;1"
        keySplines="0 0.3 0.7 1"
        :to="nextPathStr"
        @endEvent="next"
        />
    </path>
  </svg>
</template>

<script>

const POINTS_COUNT = 50
const MAX_Y = 100
const WIDTH = 600
const HEIGHT = 400
const EASE = 0.4

/**
 * 0〜1と-1〜0の乱数で交互に埋めた値配列を生成します
 * isPositiveStart = trueの場合、最初の要素は正（0〜1）になります
 */
const generateValues = (isPositiveStart = false) => {
  return new Array(POINTS_COUNT + 1)
    .fill(0)
    .map((_, index) => {
      const isPositive = (index % 2 === 0) !== isPositiveStart
      const amp = (Math.cos((index / POINTS_COUNT - 0.5) * Math.PI * 2) + 1) / 2
      return Math.random() * amp * (isPositive ? 1 : -1)
    })
}

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
      values: [],
      nextValues: [],
      sequence: 0
    }
  },
  computed: {
    pathStr () {
      return valuesToPathStr(this.values)
    },
    nextPathStr () {
      return valuesToPathStr(this.nextValues)
    }
  },
  methods: {
    next () {
      this.sequence++
      this.values = this.nextValues
      this.nextValues = generateValues(this.sequence % 2 === 0)
      this.$refs.anim.beginElement()
    }
  },
  mounted () {
    this.values = generateValues(this.sequence % 2 === 0)
    this.sequence++
    this.nextValues = generateValues(this.sequence % 2 === 0)
  }
}
</script>
