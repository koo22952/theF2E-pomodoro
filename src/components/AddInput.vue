<template>
  <section class="home-left__add">
    <input
      v-model="workList"
      placeholder="add a new mission…"
      :class="{'wordColor':breakTime}"
      @keyup.13="addList"
    />
    <i class="material-icons" @click="addList">add</i>
  </section>
</template>

<script>
  export default {
    props: ['isbreakTime'],
    data() {
      return {
        breakTime: this.isbreakTime,
        workList: '',
      }
    },
    methods: {
      addList() {
        if (this.workList === '') {
          return
        }

        this.$emit('onAddList', this.workList)
        this.workList = ''
      }
    },
  }
</script>


<style lang="scss" scoped>
  $lightColor: #ff4384;
  $lightBg: #ffedf7;
  $darkColor: #00a7ff;
  $darkBg: #e5f3ff;

  .home-left__add {
    height: 56px;
    width: 445px;
    color: $lightColor;
    position: relative;
    font-style: italic;
    font-weight: bold;
    .wordColor {
      color: $darkColor;
    }
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
      color: $lightColor;
    }
  }
</style>
