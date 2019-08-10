<template>
  <div class="home">
    <article class="home-left" :class="{'homeBreakTime':isbreakTime}">
      <AddInput :isbreakTime="isbreakTime" @onAddList="onAddList"></AddInput>
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
            <i v-if="!isWorking" class="material-icons" @click.once="onPlay('Working')">play_arrow</i>
            <i v-else class="material-icons" @click="onPause">pause</i>
          </template>
          <template v-else>
            <i v-if="!isbreak" class="material-icons" @click.once="onPlay('break')">play_arrow</i>
            <i v-else class="material-icons" @click="onPause">pause</i>
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
      <section class="home-left__listGroup">
        <div
          class="listGroup-list"
          v-for=" (item,index) in completedTodos"
          :key="item.id"
          v-if="index >= 0  && index < 3"
        >
          <template v-if="!item.isCompleted">
            <span class="listGroup-list__round" @click="onCompletTodo(item)">
              <i v-if="item.isClicked" class="material-icons">done</i>
            </span>
            <span class="listGroup-list__word">{{item.title}}</span>
            <span class="listGroup-list__btn">
              <i class="material-icons">play_circle_outline</i>
            </span>
          </template>
        </div>
        <div class="listGroup-more" :class="{'breakTime':isbreakTime}">
          <span>
            <router-link to="/Detail">more</router-link>
          </span>
        </div>
      </section>
    </article>
    <aside class="home-right">
      <nav class="home-right__nav">
        <div class="nav-icon">
          <router-link to="/Detail">
            <i class="material-icons">list</i>
          </router-link>
          <i class="material-icons">assessment</i>
          <i class="material-icons">library_music</i>
        </div>
      </nav>
      <div class="nav-title">POMODORO</div>
    </aside>
  </div>
</template>

<script>
  import AddInput from '../components/AddInput'
  import { createHash } from 'crypto';
  export default {
    name: 'home',
    components: {
      AddInput,
    },
    data() {
      return {
        WorkingSecs: 5,      //工作時間
        breakSecs: 3,       //休息時間
        isWorking: false,    //點擊工作開始
        isbreak: false,      //點擊休息開始
        isbreakTime: false,   //休息時間的時候
        timeSpacing: null,
        circlePart: 360,
        spacingPart: 0,
        todos: [
          {
            id: '1321412421414',
            title: 'one',
            isCompleted: true,
            isClicked: true,
          },
          {
            id: '132141242124',
            title: 'osssssss',
            isCompleted: false,
            isClicked: false,
          },
          {
            id: '132141244124',
            title: 'oneddgsg',
            isCompleted: false,
            isClicked: false,
          },
          {
            id: '132144214124',
            title: 'ofgdgfdfe',
            isCompleted: false,
            isClicked: false,
          }
        ],
      }
    },
    computed: {
      completedTodos() {
        let notCompletedTodos = this.todos.filter(item => {
          return !item.isCompleted
        })

        return notCompletedTodos
      },

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
      onPlay(type) {
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
      onPause() {
        clearInterval(this.timeSpacing);
        this.isWorking = false
        this.isbreak = false
      },
      onAddList(value) {
        let timestamp = new Date().getTime();
        this.todos.push(
          {
            id: timestamp,
            title: value,
            isCompleted: false,
          }
        )
      },
      onCompletTodo(item) {
        let newTodos = this.todos.map(todo => {
          if (todo.id === item.id) {
            todo.isClicked = !todo.isClicked

            setTimeout(() => {
              todo.isCompleted = !todo.isCompleted
              return {
                ...todo,
              }
            }, 200);
          } else {
            return todo
          }
        })
      }
    }
  }
</script>


<style lang="scss" scoped>
  $lightColor: #ff4384;
  $lightBg: #ffedf7;
  $darkColor: #00a7ff;
  $darkBg: #e5f3ff;
  a {
    color: #ffff;
  }
  .home {
    max-width: 1280px;
    height: 100%;
    display: flex;
    margin: 0 auto;

    &-left {
      width: 65%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      background-color: $lightBg;
      padding: 48px 0px 48px 85px;
      position: relative;
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
      .home-left__listGroup {
        .listGroup-list {
          width: 445px;
          display: flex;
          align-items: center;
          border-bottom: 1px solid rgba(0, 49, 100, 20%);
          margin-top: 10px;
          padding-bottom: 7px;
          :first-child {
            margin-top: 0px;
          }
          &__round {
            width: 24px;
            height: 24px;
            border: 2px solid #003164;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            i {
              font-size: 18px;
              font-weight: bolder;
            }
          }
          &__word {
            font-weight: bold;
            line-height: 24px;
            font-size: 16px;
            color: #003164;
            margin-left: 4px;
            margin-right: auto;
          }
          i {
            color: #003164;
          }
        }
        .listGroup-more {
          width: 445px;
          line-height: 19px;
          font-size: 16px;
          color: $lightColor;
          font-weight: bold;
          margin-top: 8px;
          text-align: right;
          span {
            cursor: pointer;
            a {
              color: #ff4384;
            }
          }
        }
        .breakTime {
          color: $darkColor;
          span {
            cursor: pointer;
            a {
              color: $darkColor;
            }
          }
        }
      }
    }
    &-right {
      width: 35%;
      height: 100%;
      padding: 48px 85px 48px 0px;
      color: white;
      background-color: #003164;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: flex-end;
      &__nav {
        display: flex;
        flex-wrap: wrap;
        width: 36px;
        .nav-icon {
          i {
            margin-bottom: 48px;
            font-size: 36px;
            cursor: pointer;
          }
        }
      }
      .nav-title {
        font-size: 24px;
        font-weight: bolder;
        writing-mode: vertical-lr;
      }
    }
  }

  // 休息時間
  .homeBreakTime {
    background-color: $darkBg;
    .home-left__add {
      color: $darkColor;
      ::placeholder {
        color: $darkColor;
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

