<template>
  <div class="root">
    <transition-group name="bars" tag="ul">
      <li class="bar" v-for="entry in sorted" :key="entry.id">
        <div class="title">{{ entry.title }}</div>
        <div class="bar-body"
          @click="entry.value += 10"
          :style="{
            width: `${entry.value / maxValue * 80}%`,
            backgroundColor: entry.color
          }">{{ entry.value }}</div>
      </li>
    </transition-group>
  </div>
</template>

<script>
export default {
  props: {
    entries: { type: Array, reqired: true }
  },
  data () {
    return {
    }
  },
  computed: {
    maxValue () {
      return Math.max(...this.entries.map(e => e.value))
    },
    sorted () {
      const entriesCopy = [...this.entries]
      return entriesCopy.sort((a, b) => b.value - a.value)
    }
  }
}
</script>

<style scoped>
.bar {
  padding: 5px;
}
.bar-body {
  height: 50px;
  display: inline-block;
  background-color: gold;
  transition: width 200ms ease;
  line-height: 50px;
  text-align: center;
}
.title {
  display: inline-block;
  width: 20%;
  line-height: 30px;
  text-align: right;
  padding: 10px;
  box-sizing: border-box;
}
ul {
  list-style: none;
  text-align: left;
  width: 80%;
  font-weight: bold;
}
.bars-move {
  transition: transform .5s;
}
</style>
