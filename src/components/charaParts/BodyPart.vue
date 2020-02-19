<template>
  <AnimBox :w="754" :h="1127" :oy="95" :r="bodyTilt">
    <AnimBox
      ref="body" img="img/parts/body.png"
      :x="79" :y="418" :w="599" :h="668" :ox="20" :oy="15"
    />
    <AnimBox
      ref="armR" img="img/parts/armR.png"
      :x="459" :y="478" :w="230" :h="425" :ox="20" :oy="15" :r="-rightArmAngle"
    />
    <AnimBox
      ref="armL" img="img/parts/armL.png"
      :x="70" :y="478" :w="230" :h="425" :ox="80" :oy="15" :r="leftArmAngle"
    />

    <HeadPart :bodyTilt="bodyTilt" :headTilt="headTilt" />

  </AnimBox>
</template>

<script>
import AnimBox from '../AnimBox'
import HeadPart from './HeadPart'
export default {
  name: 'BodyPart',
  components: { AnimBox, HeadPart },
  props: {
    bodyTilt: { type: Number, default: 0 },
    headTilt: { type: Number, default: 0 },
    leftArmAngle: { type: Number, default: 30 },
    rightArmAngle: { type: Number, default: 30 }
  },
  data () {
    return {
    }
  }
}
</script>
