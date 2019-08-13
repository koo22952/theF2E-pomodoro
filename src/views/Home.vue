<template>
  <div>
    <div v-show="!openDetail" class="home">
      <article class="home-left" :class="{'homeBreakTime':isbreakTime}">
        <AddInput :isbreakTime="isbreakTime" @onAddList="onAddList"></AddInput>
        <section v-if="tododoinging !== undefined" class="home-left__time">
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
            <template v-if="unDoneTodos.length">
              <template v-if="!isbreakTime">
                <i v-if="!isWorking" class="material-icons" @click="onPlay('Working')">play_arrow</i>
                <i v-else class="material-icons" @click="onPause">pause</i>
              </template>
              <template v-else>
                <i v-if="!isbreak" class="material-icons" @click="onPlay('break')">play_arrow</i>
                <i v-else class="material-icons" @click="onPause">pause</i>
              </template>
            </template>
            <template v-else>
              <i v-if="!isWorking" class="material-icons" @click="onPlay('Working')">play_arrow</i>
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
            v-for=" (item,index) in isCompletedTodos"
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
          <div
            class="listGroup-more"
            :class="{'breakTime':isbreakTime}"
            v-if="isCompletedTodos.length"
          >
            <span @click="onOpenDetail">more</span>
          </div>
        </section>
      </article>
      <aside class="home-right">
        <nav class="home-right__nav">
          <div class="nav-icon">
            <i class="material-icons" @click="onOpenDetail">list</i>
            <!-- <i class="material-icons">assessment</i> -->
            <!-- <i class="material-icons">library_music</i> -->
          </div>
        </nav>
        <div class="nav-title">POMODORO</div>
      </aside>
    </div>
    <div v-show="openDetail" class="detail">
      <article class="detail-left">
        <section class="detail-left__menu">
          <div class="menu-list" :class="{'breakTime':isbreakTime}">
            <i class="material-icons">list</i>
            <span class="menu-title">to-do list</span>
          </div>
        </section>
        <section v-if="tododoinging !== undefined" class="detail-left__clock">
          <div class="clock-downCircle" :class="{'clock-breakTime':isbreakTime}">
            <span class="clock-downCircle__time">
              <span>{{isbreakTime ? breakTime : pomodoroTime }}</span>
            </span>
            <span class="clock-downCircle__title">{{todoDoing.title}}</span>
            <div class="clock-upCircle" :class="{'clock-upCircleBreakTime':isbreakTime}">
              <div class="clock-upCircle__content">
                <template v-if="!isbreakTime">
                  <i v-if="!isWorking" class="material-icons" @click="onPlay('Working')">play_arrow</i>
                  <i v-else class="material-icons" @click="onPause">pause</i>
                </template>
                <template v-else>
                  <i v-if="!isbreak" class="material-icons" @click="onPlay('break')">play_arrow</i>
                  <i v-else class="material-icons" @click="onPause">pause</i>
                </template>
              </div>
            </div>
          </div>
        </section>
      </article>
      <article class="detail-right">
        <AddInput :isbreakTime="isbreakTime" @onAddList="onAddList"></AddInput>

        <section class="detail-TodoLists">
          <div class="todo">
            <div class="todo__title" @click="todoOpen = !todoOpen">
              to-do
              <i class="material-icons" v-if="todoOpen">arrow_drop_up</i>
              <i class="material-icons" v-else>arrow_drop_down</i>
            </div>
            <div class="todoAll" v-if="todoOpen">
              <div class="todo__listGroup" v-for="item in unDoneTodos" :key="item.id">
                <div class="listGroup-list">
                  <span class="listGroup-list__round" @click="onCompletTodo(item)">
                    <i v-if="item.isClicked" class="material-icons">done</i>
                  </span>
                  <span class="listGroup-list__word">{{item.title}}</span>
                  <span class="listGroup-list__btn">
                    <i class="material-icons" @click="onChangeTodo(item)">play_circle_outline</i>
                  </span>
                </div>
              </div>
            </div>
          </div>
          <div class="todo">
            <div class="todo__title" @click="doneOpen = !doneOpen">
              done
              <i class="material-icons" v-if="!doneOpen">arrow_drop_up</i>
              <i class="material-icons" v-else>arrow_drop_down</i>
            </div>
            <div class="todoAll" v-if="doneOpen">
              <div class="todo__listGroup" v-for="item in doneTodos" :key="item.id">
                <div class="listGroup-list">
                  <span class="listGroup-list__round" @click="onCompletTodo(item)">
                    <i v-if="item.isClicked" class="material-icons">done</i>
                  </span>
                  <span class="listGroup-list__word delLine">{{item.title}}</span>
                  <template v-for="i in item.doTimes">
                    <span class="finishSpan"></span>
                  </template>
                </div>
              </div>
            </div>
          </div>
        </section>
      </article>
      <aside class="detail-nav">
        <i class="material-icons detail-nav__close" @click="onOpenDetail('close')">close</i>
        <div class="detail-nav__title">
          <span>pomodoro</span>
        </div>
      </aside>
    </div>
  </div>
</template>




<script>
  import AddInput from '../components/AddInput'
  import { createHash, constants } from 'crypto';
  export default {
    name: 'home',
    components: {
      AddInput,
    },
    data() {
      return {
        doneOpen: true,
        todoOpen: true,
        openDetail: false,
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
        todoDoing: '',
        totalTodos: [
          {
            id: '1565357946245',
            title: 'two',
            isCompleted: false,
            isClicked: false,
            doTimes: 0,
            doing: false,
          },
          {
            id: '1567457946246',
            title: 'one',
            isCompleted: true,
            isClicked: true,
            doTimes: 3,
            doing: false,
          },
          {
            id: '1565457941246',
            title: 'three',
            isCompleted: false,
            isClicked: false,
            doTimes: 3,
            doing: false,
          },
          {
            id: '1505458946246',
            title: 'zxcvbnm',
            isCompleted: false,
            isClicked: false,
            doTimes: 5,
            doing: false,
          },
          {
            id: '1265457947589',
            title: 'asdfghjkl',
            isCompleted: false,
            isClicked: false,
            doTimes: 2,
            doing: true,
          },
        ],
      }
    },
    computed: {
      tododoinging() {
        return this.todoDoing = this.totalTodos.filter(todo => {
          return todo.doing
        })[0]
      },
      doneTodos() {
        let newArr = this.totalTodos.filter(item => {
          return item.isCompleted
        }).sort((x, y) => {
          return x.id - y.id
        })
        return newArr
      },
      unDoneTodos() {
        let newArr = this.totalTodos.filter(item => {
          return !item.isCompleted
        }).sort((x, y) => {
          return x.id - y.id
        })
        return newArr
      },
      isCompletedTodos() {

        let notCompletedTodos = this.totalTodos.filter(item => {
          return !item.isCompleted
        }).filter(item => {
          return !item.doing
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
      init() {
        let el = document.querySelector('#circle')
        let el2 = document.querySelector('#circle2')

        this.isWorking = false
        this.isbreak = false
        this.WorkingSecs = 5
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


        let doing = this.totalTodos.filter(todo => {
          return todo.doing
        })

        if (doing.length === 0) {
          this.$bvToast.toast(`PLEASE ADD TODO TO START!!`, {
            title: 'Message',
            toaster: 'b-toaster-top-center',
            autoHideDelay: 1000,
          })
          return
        }

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
        let s = this.totalTodos.filter(item => {
          return item.doing
        })
        if (!s.length) {
          this.totalTodos.push(
            {
              id: timestamp,
              title: value,
              isCompleted: false,
              isClicked: false,
              doTimes: 0,
              doing: true,
            }
          )
        } else {
          this.totalTodos.push(
            {
              id: timestamp,
              title: value,
              isCompleted: false,
              isClicked: false,
              doTimes: 0,
              doing: false,
            }
          )
        }
        this.$bvToast.toast(`新增成功！`, {
          title: 'Message',
          variant: "success",
          toaster: 'b-toaster-top-center',
          autoHideDelay: 500,
        })


      },
      onCompletTodo(item, type) {


        if (this.todoDoing === undefined) {
          let num = this.totalTodos.map(todo => {
            return todo.id
          }).indexOf(item.id)

          this.totalTodos[num].doing = true
        }


        this.totalTodos.map(todo => {
          if (todo.id === item.id) {
            todo.isClicked = !todo.isClicked
            todo.isCompleted = !todo.isCompleted
            return {
              ...todo,
            }
          }
          return todo
        })


        if (item.id === this.todoDoing.id) {
          let filter = this.totalTodos.filter((todo, index) => {
            if (item.id === this.todoDoing.id) {
              todo.doing = false
            }
            this.init()
            this.isbreakTime = false
            this.isWorking = false
            this.isbreak = false
            return !todo.isClicked
          })

          if (filter.length) {
            let num = this.totalTodos.map(todo => {
              return todo.id
            }).indexOf(filter[0].id)
            this.totalTodos[num].doing = true
            return
          }
        }
      },
      onChangeTodo(item) {
        if (this.todoDoing.id == item.id) {
          this.$bvToast.toast(`此項目正在進行中！`, {
            title: 'Message',
            variant: "danger",
            toaster: 'b-toaster-top-center',
            autoHideDelay: 500,
          })
          return
        }

        this.isbreakTime = false
        this.isWorking = false
        this.isbreak = false

        const len = this.unDoneTodos.length

        if (len) {
          this.init()
        }

        let s = this.totalTodos.map(todo => {
          if (this.todoDoing.id == todo.id) {
            todo.doing = false
            return {
              ...todo
            }
          }
          if (item.id === todo.id) {
            todo.doing = true
            return {
              ...todo
            }
          }
          return todo
        }).filter(todo => {
          return todo.doing
        })

        this.todoDoing = s[0]
      },
      onOpenDetail(type) {
        this.openDetail = !this.openDetail
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
    height: 100vh;
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
            cursor: pointer;
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
    &::placeholder {
      color: $darkColor !important;
    }
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

  .detail {
    max-width: 1280px;
    height: 100vh;
    display: flex;
    margin: 0 auto;
    background-color: #003164;
    padding: 45px 85px;
    color: #fff;
    font-weight: bold;
    z-index: 999999;
    &-left {
      width: 40%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      &__menu {
        .menu-list {
          color: #ff4384;
          font-size: 36px;
          line-height: 36px;
          margin-bottom: 42px;
          display: flex;
          align-items: center;
          i {
            font-size: 36px;
          }
          .menu-title {
            margin-left: 8px;
          }
        }
      }
      &__clock {
        position: absolute;
        bottom: -175px;
        width: 350px;

        .clock-downCircle {
          height: 350px;
          background-color: #ffedf7;
          border-radius: 50%;
          position: relative;
          border: 1px solid black;
          display: flex;
          flex-direction: column;
          align-items: center;

          &__time {
            font-size: 64px;
            line-height: 64px;
            color: #ff4384;
            margin-top: 58px;
            font-weight: bold;
          }
          &__title {
            color: #003164;
            font-weight: bold;
            font-size: 16px;
            line-height: 24px;
          }
          .clock-upCircle {
            height: 116px;
            width: 116px;
            background-color: #003164;
            border-radius: 50%;
            position: absolute;
            top: -58px;
            left: 50%;
            transform: translate(-50%, 0);
            display: flex;
            justify-content: center;
            align-items: center;
            &__content {
              width: 86px;
              height: 86px;
              background-color: #ff4384;
              border-radius: 50%;
              position: relative;
              text-align: center;
              cursor: pointer;
              i {
                line-height: 86px;
                font-size: 58px;
                z-index: 5;
              }
              &::before {
                content: "";
                width: 104px;
                height: 104px;
                border-radius: 50%;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                border: 2px solid #ff4384;
                z-index: -1;
              }
            }
          }
          .clock-upCircleBreakTime {
            .clock-upCircle__content {
              background-color: #00a7ff;
              &::before {
                border: 2px solid #00a7ff;
              }
            }
          }
        }
        .clock-breakTime {
          background-color: #e5f3ff;
          .clock-downCircle__time {
            color: #00a7ff;
          }
        }
      }
    }
    &-right {
      width: calc(60% - 48px);
      .detail-TodoLists {
        width: 445px;
        .todo {
          display: flex;
          margin-top: 48px;
          flex-direction: column;
          &__title {
            padding: 8px 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-size: 24px;
            background-color: rgba(255, 255, 255, 20%);
            cursor: pointer;
          }
          .todoAll {
            overflow-y: auto;
            max-height: calc((100vh - 90px - 104px - 48px - 48px - 20px) / 2);
          }
          .todoAll::-webkit-scrollbar-track {
            -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
            background-color: #f5f5f5;
          }

          .todoAll::-webkit-scrollbar {
            width: 10px;
            background-color: #f5f5f5;
          }

          .todoAll::-webkit-scrollbar-thumb {
            background-color: #000000;
            border: 2px solid #555555;
          }
          &__listGroup {
            .listGroup-list {
              display: flex;
              align-items: center;
              height: 32px;
              margin-top: 9px;
              border-bottom: 1px solid rgba(255, 255, 255, 20%);
              padding-bottom: 8px;
              &:first-child {
                margin-top: 16px;
              }
              &__round {
                width: 24px;
                height: 24px;
                border-radius: 50%;
                border: 2px solid #ffff;
                display: flex;
                justify-content: center;
                align-items: center;
                margin-right: 6px;
                cursor: pointer;
                i {
                  font-size: 18px;
                }
              }
              &__word {
                font-size: 16px;
                margin-right: auto;
              }
              .delLine {
                font-style: italic;
                position: relative;
                &::after {
                  content: "";
                  border: 1px solid rgba(255, 255, 255, 80%);
                  width: 103%;
                  position: absolute;
                  top: 50%;
                  left: 50%;
                  transform: translate(-50%, -50%);
                }
              }

              &__btn {
                display: flex;
                align-items: center;
                justify-content: center;
                i {
                  cursor: pointer;
                }
              }
              .finishSpan {
                background-color: #ffffff;
                width: 12px;
                height: 12px;
                border-radius: 50%;
                margin-left: 8px;
              }
            }
          }
        }
      }
    }
    &-nav {
      width: 48px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      &__close {
        font-size: 48px;
        cursor: pointer;
      }
      &__title {
        font-size: 24px;
        line-height: 32px;
        writing-mode: vertical-lr;
        display: flex;
        align-items: flex-end;
      }
    }
  }
</style>
