<template>
    <section class="home-left__listGroup">
        <div class="addNewProject" v-if="!lists.length">
            <i class="material-icons">youtube_searched_for</i>
            No tasks...
        </div>
        <div v-else class="listGroup-list" v-for=" (item,index) in lists" :key="item.name">
            <!-- <span class="listGroup-list__round"></span> -->
            <i class="material-icons listGroup-list__play" @click="onChangeTodo(item)">play_circle_outline</i>
            <span class="listGroup-list__word">{{item.name}}</span>
            <span class="listGroup-list__btn">
        <i class="material-icons" @click="onEditTodo(item)">edit</i>
        <i class="material-icons" @click="onDelTodo(item)">delete</i>
      </span>
        </div>
        <!-- <div class="listGroup-more" :class="{'breakTime':isbreakTime}">
          <span>more</span>
        </div>-->
    </section>
</template>

<script>
export default {
    props: ['isbreakTime', 'todoList'],
    data () {
        return {
            lists: this.todoList
        }
    },
    methods: {
        onChangeTodo (value) {
            this.$emit('changeTodo', value)
        },
        onEditTodo (value) {

        },
        onDelTodo (value) {
            this.$emit('onDelTodo',value)
        }
    }
}
</script>


<style lang="scss" scoped>
    $lightColor: #ff4384;
    $lightBg: #ffedf7;
    $darkColor: #00a7ff;
    $darkBg: #e5f3ff;

    .addNewProject {
        font-size: 24px;
        display: flex;
        align-items: center;

        i {
            font-size: 24px;
            margin-right: 5px;
        }
    }

    .home-left__listGroup {
        overflow: hidden;
        overflow-y: auto;
        height: 126px;
        width: 470px;

        &::-webkit-scrollbar-track {
            -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.03);
            background-color: #f5f5f5;
            border-radius: 10px;
        }

        &::-webkit-scrollbar {
            width: 10px;
            background-color: #f5f5f5;
            border-radius: 10px;
        }

        &::-webkit-scrollbar-thumb {
            background-color: #003164;
            border: 2px solid #555555;
            border-radius: 10px;
        }

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

            &__play {
                cursor: pointer;
            }

            &__word {
                font-weight: bold;
                line-height: 24px;
                font-size: 16px;
                color: #003164;
                margin-left: 4px;
                margin-right: auto;
            }

            &__btn {
                display: flex;
                align-items: center;

                i {
                    color: #003164;
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
            }
        }

        .breakTime {
            color: $darkColor;
        }
    }
</style>
