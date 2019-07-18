<template>
  <div class="detail">
    <article class="detail-left">
      <section class="detail-left__menu">
        <div class="menu-list">
          <i class="material-icons">list</i>
          <span class="menu-title">to-do list</span>
        </div>
        <div class="menu-list">
          <i class="material-icons">assessment</i>
          <span class="menu-title">analytics</span>
        </div>
        <div class="menu-list">
          <i class="material-icons">library_music</i>
          <span class="menu-title">ringTones</span>
        </div>
      </section>
      <section class="detail-left__clock">
        <div class="clock-downCircle">
          <div class="clock-upCircle"></div>
          <span class="clock-downCircle__time">25:00</span>
          <span class="clock-downCircle__title">the First thing to do today</span>
        </div>
      </section>
    </article>
    <article class="detail-right">
      <AddInput></AddInput>
      <TodoList></TodoList>
    </article>
    <aside class="detail-nav">
      <i class="material-icons detail-nav__close">close</i>
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
          color: rgba(255, 255, 255, 20%);
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
          .clock-upCircle {
            height: 116px;
            width: 116px;
            background-color: #003164;
            border-radius: 50%;
            position: absolute;
            top: -58px;
            left: 50%;
            transform: translate(-50%, 0);
            z-index: 1;
            &::after {
              content: "";
              width: 104px;
              height: 104px;
              // background-color: #fff;
              border-radius: 50%;
              display: block;
              margin: 0 auto;
              border: 1px solid burlywood;
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


