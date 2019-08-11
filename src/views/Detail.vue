<template></template>

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
</style>


