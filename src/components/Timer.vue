<template>
  <div>
    <transition-group name="fade" tag="div" class="container">
      <div
        v-for="(timer, idx) in timers"
        :key="idx"
        :class="{ active: timer.timerState === 'running' }"
        class="timer"
      >
        <div class="timer__time">{{ timer.formattedTime }}</div>
        <div class="timer__actions">
          <button
            v-if="timer.timerState !== 'running'"
            @click="$emit('start', idx)"
          >
            <svg width="17" height="20" viewBox="0 0 17 20">
              <path d="M0 20V0L17 10L0 20Z" fill="#9e9e9e" />
            </svg>
          </button>
          <button v-else @click="$emit('pause', idx)">
            <svg width="10" height="20" viewBox="0 0 10 20">
              <rect x="7" width="3" height="20" fill="#fff" />
              <rect width="3" height="20" fill="#fff" />
            </svg>
          </button>
          <button @click="$emit('stop', idx)">
            <svg width="20" height="20" viewBox="0 0 20 20">
              <rect width="20" height="20" fill="#9e9e9e" />
            </svg>
          </button>
        </div>
      </div>
      <button key="idx" class="add-timer" @click="$emit('add-timer')">
        <svg width="20" height="20" viewBox="0 0 20 20">
          <rect x="8.5" width="3" height="20" />
          <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" />
        </svg>
      </button>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: 'Timer',
  props: {
    timers: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 775px;
  margin: 72px auto;
  display: grid;
  grid-template-columns: 225px 225px 225px;
  grid-column-gap: 50px;
  grid-row-gap: 45px;
}
@media (max-width: 1023px) {
  .container {
    width: 500px;
    grid-template-columns: 225px 225px;
  }
}
@media (max-width: 767px) {
  .container {
    width: 225px;
    grid-template-columns: 225px;
  }
}
.timer {
  font-family: 'Gotham Pro';
  font-size: 22px;
  width: 225px;
  height: 120px;
  background-color: #696969;
  color: #9e9e9e;
  &__time {
    height: 60px;
    line-height: 60px;
    text-align: center;
    border-bottom: 1px solid #9e9e9e;
  }
  &__actions {
    height: 60px;
    width: 85px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    & button {
      cursor: pointer;
      height: 20px;
      width: 20px;
      background: transparent;
      border: none;
      outline: none;
      & svg path,
      svg rect {
        transition: all 0.2s;
      }
      &:hover {
        & svg path,
        svg rect {
          fill: #fff;
        }
      }
    }
  }
}
.add-timer {
  cursor: pointer;
  width: 225px;
  height: 120px;
  background-color: #696969;
  outline: none;
  border: none;
  color: white;
  & svg rect {
    transition: all 0.2s;
    fill: #9e9e9e;
  }
  &:hover {
    & svg rect {
      fill: #fff;
    }
  }
}
.active {
  color: #fff;
  & .timer__time {
    border-bottom: 1px solid #fff;
  }
  & svg path,
  svg rect {
    fill: #fff;
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease-out;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
