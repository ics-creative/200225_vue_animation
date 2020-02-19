<template>
  <div id="app">
    <h2>CSS + Vue.jsによるアニメーション</h2>

    <h3><a id="demo-css-1"></a>基本のCSSアニメーション</h3>
    <div class="stage" @click="moveCat1">
      <div class="note">クリックしたあたりまでスライドします</div>
      <BoxCat1 ref="cat1" />
    </div>
    <h3><a id="demo-css-2"></a>応用：アニメーションのチェイン</h3>
    <div class="stage" @click="moveCat2">
      <div class="note">クリックしたあたりまで三段跳びで移動します</div>
      <BoxCat2 ref="cat2" />
    </div>

    <h3><a id="demo-css-3"></a>応用：「溜め」を使ったアニメーション</h3>
    <div class="stage" @click="moveCat3">
      <div class="note">クリックしたあたりまで滑らかに動いてジャンプします</div>
      <BoxCat3 ref="cat3" />
    </div>

    <h3><a id="demo-css-4"></a>応用：複雑な人物のキャラクター</h3>
    <div class="stage" @click.self="tiltChara" @dblclick="jumpChara">
      <div class="note">ステージ内クリックで首を傾げて、ダブルクリックでジャンプします</div>
      <MyChara ref="chara" class="chara" />
    </div>

    <h2>SVG + Vue.jsによるアニメーション</h2>

    <h3><a id="demo-svg-1"></a>基本のSVG Path（折れ線）</h3>
    <div class="stage">
      <SvgPath />
    </div>

    <h3><a id="demo-svg-2"></a>基本のSVG Path（曲線）</h3>
    <div class="stage">
      <SvgPath2 />
    </div>

    <h3><a id="demo-svg-3"></a>JavaScript(requestAnimationFrame)によるアニメーション</h3>
    <div class="stage">
      <AnimPathJs />
    </div>

    <h3><a id="demo-svg-4"></a>SVG animate要素によるアニメーション</h3>
    <div class="stage">
      <AnimPath />
    </div>

  </div>
</template>

<script>
import BoxCat1 from './components/BoxCat1'
import BoxCat2 from './components/BoxCat2'
import BoxCat3 from './components/BoxCat3'
import MyChara from './components/MyChara.vue'
import SvgPath from './components/SvgPath'
import SvgPath2 from './components/SvgPath2'
import AnimPath from './components/AnimPath'
import AnimPathJs from './components/AnimPathJs'

export default {
  name: 'app',
  components: {
    MyChara,
    BoxCat1,
    BoxCat2,
    BoxCat3,
    SvgPath,
    SvgPath2,
    AnimPath,
    AnimPathJs
  },
  methods: {
    moveCat1 (ev) {
      // クリックされたX座標までスライドする
      this.$refs.cat1.moveTo(ev.offsetX)
    },
    async moveCat2 (ev) {
      // クリックされた座標まで三段跳びでスライド + ジャンプする
      const cat = this.$refs.cat2
      const fromX = cat.x
      const fromY = cat.y
      const toX = ev.offsetX
      const toY = ev.offsetY - 80
      // 1段目 = 20%の地点まで
      await cat.moveTo(fromX + (toX - fromX) * 0.2, fromY + (toY - fromY) * 0.2)
      // 2段目 = 50%の地点まで
      await cat.moveTo(fromX + (toX - fromX) * 0.5, fromY + (toY - fromY) * 0.5)
      // 3段目 = 100%の地点（クリックされた座標）まで
      await cat.moveTo(fromX + (toX - fromX) * 1.0, fromY + (toY - fromY) * 1.0)
    },
    moveCat3 (ev) {
      // クリックされた座標までスライド + ジャンプする
      this.$refs.cat3.moveTo(ev.offsetX, ev.offsetY)
    },
    tiltChara (ev) {
      const x = (ev.offsetX / ev.target.offsetWidth - 0.5)
      this.$refs.chara.tilt(x)
    },
    jumpChara (ev) {
      this.$refs.chara.jump(60)
    }
  }
}
</script>

<style scoped>
.stage {
  position: relative;
  width: 100%;
  height: 350px;
  border: 1px solid #aaa;
  background-color: rgb(248, 248, 247);
}
.note {
  color: #888;
  text-align: center;
  position: absolute;
  width: 100%;
  user-select: none;
}
.chara {
  left: 50%;
  filter: drop-shadow(0 0 4px rgb(56, 52, 46)) drop-shadow(0 0 4px rgb(56, 52, 46));
}
</style>
