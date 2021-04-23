<template>
  <div class="n">
    <div class="wrapper">
      <div class="full" :class="[roll ? 'roll': '', s?'stop':'']">
        <div class="each" v-for="(i,index) in items" :key="index">{{ i }}</div>
      </div>
    </div>
    <div v-show="show" class="btn" @click="begin">
      重抽
    </div>
  </div>
</template>

<script>

export default {
  name: "Number",
  props: {
    excepts: {
      default: () => [],
      type: Array
    }
  },
  data() {
    return {
      show: false,
      start: false,
      roll: false,
      items: [0],
      s: true
    }
  },
  methods: {
    stop() {
      this.s = true
    },
    begin() {
      this.start = true
      if (this.start) {
        this.reset()
        this.$nextTick(() => {
          // 应该是要用$nextTick才对，但是不符合预期，只得使用setTimeout
          setTimeout(() => {
            this.random()
          }, 30)
        })
      } else {
        this.random()
      }
    },
    random() {
      this.show = false
      let arr = [...Array(1200).keys()].filter(i => this.excepts.indexOf(i) === -1);
      const result = arr.sort(function () {
        return Math.random() - 0.5;
      }).slice(300)
      this.$emit('change', result[result.length - 1])
      const {items} = this
      this.items = [items[0], ...result]
      this.roll = true
      this.s = false
      this.interval = setInterval(() => {
        if (this.s) {
          clearInterval(this.interval)
          setTimeout(() => {
            this.show = true
            this.$emit('ok')
          }, 280)
        }
      }, 3500)
    },
    reset() {
      const {items} = this
      if (items[items.length - 1] !== 0) this.$emit('reset', items[items.length - 1])
      if (items.length !== 1) {
        this.$set(this.items, 0, items[items.length - 1])
      }
      this.roll = false
    },
  }
}
</script>

<style lang="sass" scoped>
.btn
  padding: 6px 12px
  cursor: pointer

$eachHeight: 60px
.n
  display: flex
  flex-direction: column
  align-items: center
  height: $eachHeight + 40px

  .wrapper
    width: $eachHeight
    height: $eachHeight
    border: 1px solid #2c3e50
    overflow: hidden

    .full
      animation: none

    .full.roll
      transform: translateY(calc(-100% + 60px))
      animation: move 3.5s forwards steps(200, end)
      animation-iteration-count: infinite

    .full.roll.stop
      animation-iteration-count: 1

    .each
      height: $eachHeight
      width: $eachHeight
      display: flex
      align-items: center
      justify-content: center

@keyframes move
  0%
    transform: translateY(0)
  100%
    transform: translateY(calc(-100% + 60px))


</style>
