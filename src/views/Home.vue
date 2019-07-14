<template>
  <div class="home">
    <article class="home-left" :class="{'breakTime':isbreakTime}">
      <section class="home-left__add">
        <input placeholder="add a new mission…" />
        <i class="material-icons">add</i>
      </section>
      <Time @breckTime="(val)=>{isbreakTime = val}"></Time>
      <ListGroup :isbreakTime="isbreakTime"></ListGroup>
    </article>
    <aside class="home-right">
      <nav class="home-right__nav">
        <div class="nav-icon">
          <i class="material-icons">list</i>
          <i class="material-icons">assessment</i>
          <i class="material-icons">library_music</i>
        </div>
      </nav>
      <div class="nav-title">POMODORO</div>
    </aside>
  </div>
</template>

<script>
  import ListGroup from '../components/ListGroup'
  import Time from '../components/Time'

  export default {
    name: 'home',
    components: {
      ListGroup,
      Time
    },
    data() {
      return {
        isbreakTime: false
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
      &__add {
        height: 56px;
        width: 445px;
        color: $lightColor;
        position: relative;
        font-style: italic;
        font-weight: bold;
        input {
          height: 100%;
          width: 100%;
          padding: 0px 16px;
          font-size: 16px;
          border: none;
          color: $lightColor;
          font-style: italic;
          font-weight: bold;
          text-transform: uppercase;
        }
        i {
          position: absolute;
          cursor: pointer;
          top: 50%;
          right: 16px;
          transform: translate(0, -50%);
        }
        ::placeholder {
          font-size: 16px;
          font-weight: bold;
          font-style: italic;
          text-transform: uppercase;
          color: $lightColor;
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
  .breakTime {
    background-color: $darkBg;
    .home-left__add {
      color: $darkColor;
      ::placeholder {
        color: $darkColor;
      }
    }
  }
</style>

