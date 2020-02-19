<template>
  <AnimBox ref="legs"
    :x="320" :y="1012" :w="170" :h="208" :oy="95"
  >
    <AnimBox
      ref="legR" img="img/parts/leg.png"
      :x="0" :y="0" :w="83" :h="208" :ox="50" :oy="0" :r="rightLegAngle"
    />
    <AnimBox
      ref="legL" img="img/parts/leg.png"
      :x="85" :y="0" :w="83" :h="208" :ox="50" :oy="0" :r="-leftLegAngle"
    />
  </AnimBox>
</template>

<script>
import AnimBox from '../AnimBox'
export default {
  name: 'HeadPart',
  components: { AnimBox },
  props: {
    leftLegAngle: { type: Number, default: 5 },
    rightLegAngle: { type: Number, default: 5 }
  },
  data () {
    return {
    }
  }
}
</script>
