<template>
  <div class="detail">
    <article class="detail-left">
      <section class="detail-left__menu">
        <div class="menu-list">
          <i class="material-icons">list</i>
          <span class="menu-title">to-do list</span>
        </div>
        <!-- <div class="menu-list">
          <i class="material-icons">assessment</i>
          <span class="menu-title">analytics</span>
        </div>
        <div class="menu-list">
          <i class="material-icons">library_music</i>
          <span class="menu-title">ringTones</span>
        </div>-->
      </section>
      <section class="detail-left__clock">
        <div class="clock-downCircle">
          <span class="clock-downCircle__time">25:00</span>
          <span class="clock-downCircle__title">the First thing to do today</span>
          <div class="clock-upCircle">
            <div class="clock-upCircle__content">
              <i class="material-icons">play_arrow</i>
            </div>
          </div>
        </div>
      </section>
    </article>
    <article class="detail-right">
      <AddInput></AddInput>
      <TodoList></TodoList>
    </article>
    <aside class="detail-nav">
      <i class="material-icons detail-nav__close" @click="onGoBack">close</i>
      <div class="detail-nav__title">
        <span>pomodoro</span>
      </div>
    </aside>
  </div>
</template>

<script>
  import AddInput from '../components/AddInput'
  import TodoList from '../components/TodoList'
  export default {
    name: 'Detail',
    components: {
      AddInput,
      TodoList
    },
    methods: {
      onGoBack() {
        this.$router.push('/')
      }
    }
  }
</script>


<style lang="scss" scoped>
  .detail {
    max-width: 1280px;
    height: 100%;
    display: flex;
    margin: 0 auto;
    background-color: #003164;
    padding: 45px 85px;
    color: #fff;
    font-weight: bold;
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
              i {
                line-height: 86px;
                font-size: 58px;
              }
              &::after {
                content: "";
                width: 104px;
                height: 104px;
                border-radius: 50%;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                border: 2px solid #ff4384;
              }
            }
          }
        }
      }
    }
    &-right {
      width: calc(60% - 48px);
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


