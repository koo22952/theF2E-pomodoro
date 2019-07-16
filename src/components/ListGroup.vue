<template>
  <section class="home-left__listGroup">
    <div class="listGroup-list" v-for=" item in 3">
      <span class="listGroup-list__round"></span>
      <span class="listGroup-list__word">the second thing to do today</span>
      <span class="listGroup-list__btn">
        <i class="material-icons">play_circle_outline</i>
      </span>
    </div>
    <div class="listGroup-more" :class="{'breakTime':isbreakTime}">
      <span>more</span>
    </div>
  </section>
</template>

<script>
  export default {
    props: ["isbreakTime"],
    data() {
      return {

      }
    }
  }
</script>


<style lang="scss" scoped>
  $lightColor: #ff4384;
  $lightBg: #ffedf7;
  $darkColor: #00a7ff;
  $darkBg: #e5f3ff;

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
      }
    }
    .breakTime {
      color: $darkColor;
    }
  }
</style>
