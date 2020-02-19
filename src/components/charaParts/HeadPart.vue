<template>
  <AnimBox ref="headGr"
    :w="754" :h="650" :oy="70" :r="headTilt"
  >
    <AnimBox
      ref="head" img="img/parts/head.png"
      :x="125" :y="0" :w="493" :h="465"
    />
    <AnimBox
      ref="hairR" img="img/parts/hairR.png"
      :x="500" :y="0" :w="258" :h="649" :ox="20" :oy="15" :r="-(headTilt + bodyTilt) * 0.8" :delay="300"
    />
    <AnimBox
      ref="hairL" img="img/parts/hairL.png"
      :x="0" :y="0" :w="258" :h="649" :ox="80" :oy="15" :r="-(headTilt + bodyTilt) * 0.8" :delay="300"
    />
  </AnimBox>
</template>

<script>
import AnimBox from '../AnimBox'
export default {
  name: 'HeadPart',
  components: { AnimBox },
  props: {
    headTilt: { type: Number, default: 30 },
    bodyTilt: { type: Number, default: 0 }
  },
  data () {
    return {
    }
  }
}
</script>
