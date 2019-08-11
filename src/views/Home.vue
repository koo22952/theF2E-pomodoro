<template>
  <div class="home">
    <article class="home-left" :class="{'homeBreakTime':isbreakTime}">
      <AddInput :isbreakTime="isbreakTime" @onAddList="onAddList"></AddInput>
      <section class="home-left__time" v-if="!todoDoing.isCompleted">
        <div class="time-title">
          <div class="time-title__round" @click="onCompletTodo(todoDoing,'todoDoing')">
            <i v-if="todoDoing.isClicked" class="material-icons">done</i>
          </div>
          <div class="time-title__context">
            <span class="context__up">{{todoDoing.title}}</span>
            <template>
              <div class="context__down">
                <template v-for="i in todoDoing.doTimes">
                  <span class="finishSpan"></span>
                </template>
                <!-- <div class="Ring">
                  <svg width="12" height="12" class="SVG">
                    <circle
                      cx="6"
                      cy="6"
                      r="3"
                      fill="none"
                      stroke-width="6"
                      stroke-dasharray="18.5"
                      stroke-dashoffset="18.5"
                      class="Rate"
                    />
                  </svg>
                </div>-->
                <div class="Ring" v-show="!isbreakTime">
                  <svg id="circleSvg2">
                    <circle
                      id="circle2"
                      cx="6"
                      cy="6"
                      r="3"
                      stroke-width="6"
                      stroke="#ff4384"
                      stroke-dashoffset="360%"
                    />
                  </svg>
                </div>
              </div>
            </template>
          </div>
        </div>
        <div class="time-content" :class="{'breakTime':isbreakTime}">
          <span>{{isbreakTime ? breakTime : pomodoroTime }}</span>
        </div>
      </section>
      <template v-else>
        <span class="home-left__noAnyDoing">PLEASE ADD TODO TO START...</span>
      </template>
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
              <i class="material-icons" @click="onChangeTodo(item)">play_circle_outline</i>
            </span>
          </template>
        </div>
        <div class="listGroup-more" :class="{'breakTime':isbreakTime}" v-if="completedTodos.length">
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
        circlePart2: 360,
        spacingPart: 0,
        spacingPart2: 0,
        todos: [
          {
            id: '1565457946245',
            title: 'one',
            isCompleted: false,
            isClicked: false,
            doTimes: 0,
          },
        ],
        todoDoing: {
          id: '1565457947589',
          title: 'ofgddfe',
          isCompleted: false,
          isClicked: false,
          doTimes: 2,
        },
      }
    },
    computed: {
      completedTodos() {
        let notCompletedTodos = this.todos.filter(item => {
          return !item.isCompleted
        }).sort((x, y) => {
          return x.id - y.id
        })

        if (this.todoDoing.isCompleted) {
          notCompletedTodos.length == 0 ? this.todoDoing = this.todos[0] : this.todoDoing = notCompletedTodos[0]
          notCompletedTodos.splice(0, 1)
        }



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
      init() {
        let el = document.querySelector('#circle')
        let el2 = document.querySelector('#circle2')

        this.isWorking = false
        this.isbreak = false
        this.WorkingSecs = 15
        this.breakSecs = 3
        this.spacingPart = 0
        this.spacingPart2 = 0
        this.circlePart = 360
        this.circlePart2 = 360
        el.style.strokeDashoffset = '360%'
        el2.style.strokeDashoffset = '360%'

        clearInterval(this.timeSpacing);
      },
      onPlay(type) {
        let secs = [`${type}Secs`]
        let el = document.querySelector('#circle')
        let el2 = document.querySelector('#circle2')
        let spacing = 295 / this[secs]
        let spacing2 = 153 / this[secs]
        let circle = 360
        let circle2 = 360
        let progressBarColor
        this.circlePart !== 360 ? circle = this.circlePart : ''
        this.circlePart2 !== 360 ? circle2 = this.circlePart2 : ''
        this.spacingPart === 0 ? this.spacingPart = spacing : spacing = this.spacingPart
        this.spacingPart2 === 0 ? this.spacingPart2 = spacing2 : spacing2 = this.spacingPart2



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
            type === "Working" ? this.todoDoing.doTimes = this.todoDoing.doTimes + 1 : ''
            this.isbreakTime = !this.isbreakTime
            this.init()

            return
          }
          circle = circle - spacing
          circle2 = circle2 - spacing2
          this.circlePart = circle
          this.circlePart2 = circle2
          el.style.strokeDashoffset = `calc(${circle}%)`
          el2.style.strokeDashoffset = `calc(${circle2}%)`

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

        if (this.todoDoing.isCompleted) {
          this.todoDoing = {
            id: timestamp,
            title: value,
            isCompleted: false,
            isClicked: false,
            doTimes: 0,
          }
          return
        }

        this.todos.push(
          {
            id: timestamp,
            title: value,
            isCompleted: false,
            isClicked: false,
            doTimes: 0,
          }
        )
      },
      onCompletTodo(item, type) {

        this.init()

        switch (type) {
          case 'todoDoing':
            this.todoDoing.isClicked = true
            setTimeout(() => {
              this.isbreakTime = false
              this.todoDoing.isCompleted = true
              this.todos.push(this.todoDoing)
              if (!this.todos[0].isCompleted) {

                this.todoDoing = this.todos[0]
                this.todos.splice(0, 1)
              }
            }, 500);

            break
          default:
            let newTodos = this.todos.map(todo => {
              if (todo.id === item.id) {
                todo.isClicked = !todo.isClicked

                setTimeout(() => {
                  todo.isCompleted = !todo.isCompleted
                  return {
                    ...todo,
                  }
                }, 500);
              } else {
                return todo
              }
            })
        }

      },
      onChangeTodo(item) {
        this.init()


        this.todos.push(this.todoDoing)
        this.todoDoing = item

        let num = this.todos.map(n => {
          return n.id
        }).indexOf(item.id)

        this.todos.splice(num, 1)

      }
    }
  }
</script>


<style lang="scss" scoped>
  $lightColor: #ff4384;
  $lightBg: #ffedf7;
  $darkColor: #00a7ff;
  $darkBg: #e5f3ff;

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
            cursor: pointer;
            width: 48px;
            height: 48px;
            border: 2px solid #003164;
            border-radius: 50%;
            margin-right: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            i {
              font-size: 32px;
            }
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
              display: inline-flex;
              margin-left: 1.2px;
              margin-top: 8px;
              svg {
                display: block;
                transform: rotate(-90deg);
              }
              .finishSpan {
                background-color: #003164;
                width: 12px;
                height: 12px;
                border-radius: 50%;
                margin-right: 5px;
              }
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
          &__btn {
            i {
              cursor: pointer;
            }
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
      .home-left__noAnyDoing {
        font-size: 24px;
        color: #003164;
        font-weight: 600;
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

  // 小圓
  .Ring {
    position: relative;
    display: inline-block;
    left: auto;
    top: auto;
    width: 12px;
    height: 12px;
    vertical-align: bottom;
    &::before {
      content: "";
      position: absolute;
      display: block;
      width: 100%;
      height: 100%;
      border: 1px solid #ff4384;
      box-sizing: border-box;
      border-radius: 12px;
    }
    #circleSvg2 {
      position: absolute;
      top: 50%;
      transform: translate(0%, -50%) rotate(-90deg);
      width: 12px;
      height: 12px;
      stroke-dasharray: 360%;
      stroke-dashoffset: 360%;
      fill: none;
    }

    .SVG {
      margin-left: 0px;
      margin-top: 0px;
      position: relative;
      > .Rate {
        stroke: #ff4384;
      }
    }
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

