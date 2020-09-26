<template>
  <div id="app">
    <Timer
      :timers="timers"
      @start="startTimer"
      @pause="pauseTimer"
      @stop="stopTimer"
      @add-timer="addTimer"
    />
  </div>
</template>

<script>
import Timer from '@/components/Timer'
export default {
  name: 'App',
  components: {
    Timer
  },
  data() {
    return {
      timers: [
        {
          timerState: 'stopped',
          currentTimer: 0,
          formattedTime: '0',
          interval: null
        }
      ]
    }
  },
  methods: {
    tick(idx) {
      this.timers[idx].interval = setInterval(() => {
        this.timers[idx].currentTimer++
        this.timers[idx].formattedTime = this.formatTime(
          this.timers[idx].currentTimer
        )
      }, 1000)
    },
    startTimer(idx) {
      if (this.timers[idx].timerState !== 'running') {
        this.tick(idx)
        this.timers[idx].timerState = 'running'
      }
    },
    pauseTimer(idx) {
      if (this.timers[idx].timerState !== 'stopped') {
        clearInterval(this.timers[idx].interval)
        this.timers[idx].timerState = 'stopped'
      }
    },
    stopTimer(idx) {
      clearInterval(this.timers[idx].interval)
      this.timers[idx].timerState = 'stopped'
      this.timers[idx].formattedTime = this.formatTime(
        (this.timers[idx].currentTimer = 0)
      )
    },
    addTimer() {
      this.timers.push({
        timerState: 'stopped',
        currentTimer: 0,
        formattedTime: '0',
        interval: null
      })
    },
    formatTime(seconds) {
      let measuredTime = new Date(null)
      measuredTime.setSeconds(seconds)
      let time = measuredTime.toISOString()
      if (seconds < 10) {
        return time.substr(18, 1)
      }
      if (seconds < 60) {
        return time.substr(17, 2)
      }
      if (seconds < 600) {
        return time.substr(15, 4)
      }
      if (seconds < 3600) {
        return time.substr(14, 5)
      }
      if (seconds < 36000) {
        return time.substr(12, 7)
      } else return time.substr(11, 8)
    }
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: 'Gotham Pro';
  src: url(./fonts/GothamPro.woff);
}
*,
*::before,
*::after {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  background-color: #353638;
}
</style>
