<template>
  <div>
    <section class="home-left__time">
      <div class="time-title">
        <div class="time-title__round"></div>
        <div class="time-title__context">
          <span class="context__up">the First thing to do today</span>
          <span class="context__down"></span>
        </div>
      </div>
      <div class="time-content" :class="{'breakTime':isbreakTime}">
        <span>{{isbreakTime ? breakTime : pomodoroTime }}</span>
      </div>
    </section>
    <!-- 點開始 -->
    <!-- beginTime -->
    <!-- 點休息開始 -->
    <!-- breakBeginTime -->
    <section
      class="clock"
      :class="{'beginTime':isWorking,'breakClock':isbreakTime,'breakBeginTime':isbreak}"
    >
      <div class="clock-btn">
        <template v-if="!isbreakTime">
          <i
            v-if="!isWorking"
            class="material-icons"
            @click.once="handleClick('Working')"
          >play_arrow</i>
          <i v-else class="material-icons" @click="handlePause">pause</i>
        </template>
        <template v-else>
          <i v-if="!isbreak" class="material-icons" @click.once="handleClick('break')">play_arrow</i>
          <i v-else class="material-icons">pause</i>
        </template>
      </div>
    </section>
    <svg id="circleSvg">
      <circle
        id="circle"
        cx="50%"
        cy="50%"
        r="220"
        stroke-width="25"
        stroke="#ff4384"
        stroke-dashoffset="360%"
      />
    </svg>
  </div>
</template>

<script>
  import { setTimeout } from 'timers';

  export default {
    data() {
      return {
        WorkingSecs: 15,      //工作時間
        breakSecs: 3,       //休息時間
        isWorking: false,    //點擊工作開始
        isbreak: false,      //點擊休息開始
        isbreakTime: false,   //休息時間的時候
        timeSpacing: null,
        circlePart: 360,
        spacingPart: 0,
      }
    },
    computed: {
      pomodoroTime() {
        let secs = this.WorkingSecs
        let min = Math.floor(secs / 60);
        let sec = parseInt(secs - (min * 60));

        while (min.toString().length < 2) { min = '0' + min; }
        while (sec.toString().length < 2) { sec = '0' + sec; }

        return min + ':' + sec;
      },
      breakTime() {
        let secs = this.breakSecs
        let min = Math.floor(secs / 60);
        let sec = parseInt(secs - (min * 60));

        while (min.toString().length < 2) { min = '0' + min; }
        while (sec.toString().length < 2) { sec = '0' + sec; }

        return min + ':' + sec;
      }
    },

    methods: {
      handleClick(type, s) {
        let secs = [`${type}Secs`]
        let el = document.querySelector('#circle')
        let spacing = 295 / this[secs]
        let circle = 360
        let progressBarColor
        this.circlePart !== 360 ? circle = this.circlePart : ''
        this.spacingPart === 0 ? this.spacingPart = spacing : spacing = this.spacingPart

        switch (type) {
          case 'Working':
            progressBarColor = "#ff4384"
            setTimeout(() => {
              this.isWorking = true
            }, 1000)
            break
          case 'break':
            progressBarColor = "#00A7FF"
            setTimeout(() => {
              this.isbreak = true
            }, 1000)

            break
        }

        el.style.stroke = progressBarColor

        this.timeSpacing = setInterval(() => {
          if (this[secs] === 0) {
            this.isbreakTime = !this.isbreakTime
            this.isWorking = false
            this.isbreak = false
            this.WorkingSecs = 15
            this.breakSecs = 3
            this.spacingPart = 0
            this.circlePart = 360
            this.$emit('breckTime', this.isbreakTime)
            el.style.strokeDashoffset = '360%'
            clearInterval(this.timeSpacing);
            return
          }
          circle = circle - spacing
          this.circlePart = circle
          el.style.strokeDashoffset = `calc(${circle}%)`
          this[secs]--
        }, 1000)

      },
      handlePause() {
        clearInterval(this.timeSpacing);
        this.isWorking = false
        this.isbreak = false
      }

    }
  }

</script>

<style lang="scss" scoped>
  $lightColor: #ff4384;
  $lightBg: #ffedf7;
  $darkColor: #00a7ff;
  $darkBg: #e5f3ff;

  .home-left__time {
    .time-title {
      display: flex;
      &__round {
        width: 48px;
        height: 48px;
        border: 2px solid #003164;
        border-radius: 50%;
        margin-right: 16px;
      }
      &__context {
        display: flex;
        flex-direction: column;
        .context__up {
          font-size: 24px;
          font-weight: bold;
          line-height: 28px;
          color: #003164;
          text-transform: uppercase;
        }
        .context__down {
          width: 12px;
          height: 12px;
          border: 1px solid $lightColor;
          margin-top: 8px;
          border-radius: 50%;
        }
      }
    }
    .time-content {
      height: auto;
      font-size: 176px;
      font-weight: bold;
      line-height: 176px;
      color: $lightColor;
      span {
        width: 454px;
      }
    }
  }

  // 時鐘
  .clock {
    position: absolute;
    top: 50%;
    right: calc(-220px);
    transform: translate(0%, -50%);
    width: 440px;
    height: 440px;
    background: $lightColor;
    border-radius: 50%;
    z-index: 1;

    &::after {
      content: "";
      border-radius: 50%;
      border: 4px solid $lightColor;
      width: 470px;
      height: 470px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 1;
    }
    &-btn {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      background: #fff;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      z-index: 2;
      i {
        color: $lightColor;
        font-size: 60px;
      }
    }
  }

  // 點擊開始
  .beginTime {
    background: #fff;
    border: 4px solid $lightColor;
    .clock-btn {
      background: $lightColor;
      i {
        color: #fff;
      }
    }
  }

  // 點擊休息開始
  .breakBeginTime {
    background: #fff !important;
    border: 4px solid $darkColor;
    .clock-btn {
      background: $darkColor;
      i {
        color: #fff;
      }
    }
  }

  // 進度條
  #circleSvg {
    position: absolute;
    top: 50%;
    right: -235px;
    transform: translate(0%, -50%) rotate(-90deg);
    width: 470px;
    height: 470px;
    stroke-dasharray: 360%;
    stroke-dashoffset: 360%;
    fill: none;
  }

  // 休息的顏色
  .breakTime {
    color: $darkColor !important;
  }

  .breakClock {
    background-color: $darkColor;
    &::after {
      content: "";
      border: 4px solid $darkColor;
    }
    i {
      color: $darkColor;
    }
  }
</style>
