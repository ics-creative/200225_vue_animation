<template>
  <AnimBox :sx="0.18" :sy="0.18" :x="-754 / 2" :y="-1221 + 340" :w="754" :h="1221" :ox="50" :oy="100" ref="chara">
    <LegPart :leftLegAngle="leftLegAngle" :rightLegAngle="rightLegAngle" />
    <BodyPart :bodyTilt="bodyTilt" :headTilt="headTilt" :leftArmAngle="armAngle" :rightArmAngle="armAngle" />
  </AnimBox>
</template>

<script>
import AnimBox from './AnimBox'
import BodyPart from './charaParts/BodyPart'
import LegPart from './charaParts/LegPart'
export default {
  name: 'MyChara',
  components: { AnimBox, BodyPart, LegPart },
  props: {
  },
  data () {
    return {
      bodyTilt: 0,
      headTilt: 0,
      armAngle: 0,
      leftLegAngle: 0,
      rightLegAngle: 0
    }
  },
  methods: {
    tilt (x) {
      const MAXANGLE = 100
      const angle = x * MAXANGLE
      this.bodyTilt = angle / 2
      this.headTilt = angle / 2
      this.armAngle = angle
      this.leftLegAngle = angle > 0 ? 0 : -angle
      this.rightLegAngle = angle < 0 ? 0 : angle
    },
    async jump (height = 100) {
      const chara = this.$refs.chara
      this.tilt(0)
      await chara.animTo({ y: 20, sy: 0.8 }, 300) // ジャンプの「溜め」
      this.armAngle = 60
      this.leftLegAngle = -10
      this.rightLegAngle = -10
      await chara.animTo({ y: -height, sy: 1.2 }, 700) // ジャンプ往き+伸び
      await chara.animTo({ y: 0, sy: 0.8 }, 700) // ジャンプ戻り+縮み
      this.tilt(0)
      await chara.animTo({ sy: 1.0 }, 300) // 縮みを戻す
    }
  }
}
</script>
