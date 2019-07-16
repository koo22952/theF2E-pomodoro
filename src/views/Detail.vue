<template>
  <div class="detail">
    <article class="detail-left">
      <section class="detail-left__menu">
        <div class="menu">
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
        </div>
      </section>
      <section class="detail-left__clock">
        <div class="clock-upCircle"></div>
        <div class="clock-downCircle">
          <span class="clock-downCircle__time"></span>
          <span class="clock-downCircle__title"></span>
        </div>
      </section>
    </article>
    <article class="detail-right">
      <AddInput></AddInput>
      <section class="detail-right__todoList">
        <div class="todoList-header">
          <span class="todoList-header__title">to-do</span>
          <i class="todoList-header__iconUp"></i>
          <i class="todoList-header__icondown"></i>
        </div>
        <div class="todoList-listGroup">
          <div class="todoList-listGroup__list">
            <input class="list-radio" type="radio" />
            <span class="list-title">thevfirst thing to do today</span>
            <i class="list-play"></i>
          </div>
        </div>
      </section>
      <section class="detail-right__todoListDone">
        <div class="todoList-header">
          <span class="todoList-header__title"></span>
          <i class="todoList-header__iconUp"></i>
          <i class="todoList-header__icondown"></i>
        </div>
        <div class="todoList-listGroup">
          <div class="todoList-listGroup__list">
            <input class="list-radio" type="radio" />
            <span class="list-title del">mockup thing to do today</span>
            <i class="list-history"></i>
          </div>
        </div>
      </section>
    </article>
    <aside class="detail-nav">
      <div class="detail-nav__close">X</div>
      <div class="detail-nav__title">pomodoro</div>
    </aside>
  </div>
</template>

<script>
  import AddInput from '../components/AddInput'

  export default {
    name: 'Detail',
    components: {
      AddInput
    }
  }
</script>



