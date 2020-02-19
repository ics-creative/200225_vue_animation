<template>
  <svg xmlns="http://www.w3.org/2000/svg" width="600" height="400" viewBox="0 0 600 400" >
    <path :d="pathStr" stroke="#888888" stroke-width="2" fill="none">
    </path>
  </svg>
</template>

<script>

const POINTS_COUNT = 20
const MAX_Y = 100
const WIDTH = 600
const HEIGHT = 400

/**
 * 0〜1と-1〜0の乱数で交互に埋めた値配列を生成します
 */
const generateValues = () => {
  return new Array(POINTS_COUNT + 1)
    .fill(0)
    .map((_, index) => Math.random() * ((index % 2) ? 1 : -1))
}

/**
 * 値配列を元にパスを描画するための文字列を生成して返します
 */
const valuesToPathStr = (values) => {
  if (!values.length) { return 'M0,0' }
  // 画面サイズの設定に合わせて波の座標を拡大
  const points = values.map((y, x) => ({ x: x / (POINTS_COUNT - 1) * WIDTH, y: y * MAX_Y + HEIGHT / 2 }))
  const p0 = points.shift()
  // 全ての座標を折れ線で結ぶパス文字列を生成して返却
  return `M${p0.x},${p0.y} L` + points.map(p => `${p.x},${p.y}`).join(' ')
}

export default {
  data () {
    return {
      values: []
    }
  },
  computed: {
    pathStr () {
      return valuesToPathStr(this.values)
    }
  },
  methods: {
    next () {
      this.values = generateValues()
    }
  },
  mounted () {
    this.next()
    window.setInterval(this.next, 2000) // 2秒ごとに新しい波形を作る
  }
}
</script>
