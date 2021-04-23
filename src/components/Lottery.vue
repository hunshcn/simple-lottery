<template>
  <div class="container">
    <div class="number-wrapper">
      <Number :ref="i" v-for="i in 10" :key="i" @change="onChange" @reset="onReset" :excepts="excepts" @ok="onOK"/>
    </div>
    <div style="position: relative">
    <div class="btns" :class="showBtn && !start? '': 'hidden'">
      <div class="btn start" @click="begin(1, true)">Begin</div>
    </div>
    <div class="btns" :class="showBtn && start ? '': 'hidden'">
      <div class="btn save" @click="save">Next</div>
    </div>
    <div class="btns" :class="running ? '': 'hidden'">
      <div class="btn start" @click="stop(1, true)">Stop</div>
    </div>
  </div></div>
</template>

<script>
import Number from "./Number"

export default {
  name: "Lottery",
  components: {Number},
  data() {
    return {
      old_result: [],
      result: [],
      counter: 0,
      showBtn: true,
      start: false,
      running: false
    }
  },
  computed: {
    excepts() {
      return [...this.old_result, ...this.result]
    }
  },
  methods: {
    stop(i, c) {
      if (!this.running && c)
        return
      this.running = false
      this.$refs[i][0].stop()
      if (i === 10) {
        return
      }
      setTimeout(() => {
        this.stop(i + 1)
      }, 80)
    },
    begin(i, c) {
      if (!this.showBtn && c)
        return
      this.showBtn = false
      this.$refs[i][0].begin()
      if (i === 10) {
        this.start = true
        this.running = true
        return
      }
      setTimeout(() => {
        this.begin(i + 1)
      }, 80)
    },
    save() {
      if (!this.showBtn)
        return
      const {result} = this
      this.$emit('ok', [...result])
      this.old_result.push(...result)
      for (let i = 1; i <= 10; i++) {
        this.$refs[i][0].reset()
      }
      this.start = false
    },
    onChange(n) {
      this.result.push(n)
    },
    onReset(n) {
      const r = []
      for (const i of this.result) {
        if (i !== n)
          r.push(i)
      }
      this.result = r
    },
    onOK() {
      this.counter += 1
      if (this.counter === 10) {
        this.counter = 0
        this.showBtn = true
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.container
  display: flex
  flex-direction: column
  align-items: center

.btns
  position: absolute
  left: 50%
  width: 200px
  display: flex
  justify-content: center
  align-items: center
  transform: translate(-50%, 0)
  opacity: 1
  transition: 0.15s all ease-in-out

.btns.hidden
  transform: translate(-50%, 80%)
  opacity: 0

.btn
  display: flex
  justify-content: center
  align-items: center
  height: 48px
  width: 88px
  border: 1px #2c3e50 solid
  margin-top: 16px
  cursor: pointer

.btns.hidden .btn
  cursor: default

.number-wrapper
  width: 800px
  display: flex
  justify-content: space-between
  margin: auto
</style>
