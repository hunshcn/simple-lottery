<template>
  <div id="app">
    <div class="result-items" :style="{height: result.length * 60 + 'px'}">
      <div class="rt" v-for="(item, index) in result" :key="index">
        <div class="label">
          第{{ '一二三四五六七八九十'[index] }}轮抽奖：
        </div>
        <div class="result">
          <div class="i" v-for="i in item" :key="i">{{ i }}</div>
        </div>
      </div>
    </div>
    <Lottery @ok="onOK"/>
  </div>
</template>

<script>
import Lottery from "./components/Lottery"

export default {
  name: 'App',
  components: {
    Lottery
  },
  data() {
    return {
      result: [[910, 705, 73, 1014, 780, 162, 355, 1176, 899, 376]],
    }
  },
  methods: {
    onOK(items) {
      this.result.push(items)
    }
  }
}
</script>

<style lang="sass" scoped>
#app
  font-family: Avenir, Helvetica, Arial, sans-serif

  -webkit-font-smoothing: antialiased

  -moz-osx-font-smoothing: grayscale

  color: #2c3e50
  margin-top: 60px

.result-items
  width: 800px
  margin: auto
  transition: 0.3s height ease

  .rt
    opacity: 0
    animation: fadeInAnimation ease 0.5s forwards 1 0.3s
    height: 60px

    .label
      display: inline-block

    .result
      display: inline-block

      .i
        display: inline-block
        font-size: 16px
        width: 3em
        text-align: right

      .i + .i
        margin-left: 12px

@keyframes fadeInAnimation
  0%
    opacity: 0
  100%
    opacity: 1


</style>
