<template>
    <div class="home">
        <article class="home-left" :class="{'breakTime':isbreakTime}">
            <AddInput :isbreakTime="isbreakTime" @onAddList="onAddList"></AddInput>
            <Time @breckTime="(val)=>{isbreakTime = val}" :todoNow="todo"></Time>
            <ListGroup :isbreakTime="isbreakTime" :todoList="todoList" @changeTodo="changeTodo" @onDelTodo="onDelTodo"></ListGroup>
        </article>
        <aside class="home-right">
            <nav class="home-right__nav">
                <div class="nav-icon">
                    <!-- <i class="material-icons">list</i>
                    <i class="material-icons">assessment</i>
                    <i class="material-icons">library_music</i>-->
                </div>
            </nav>
            <div class="nav-title">POMODORO</div>
        </aside>
        <b-modal ref="my-modal" hide-footer title="Using Component Methods">
            <div class="d-block text-center">
                <h3>Hello From My Modal!</h3>
            </div>
            <b-button class="mt-3" variant="outline-danger" block @click="hideModal">Close Me</b-button>
            <b-button class="mt-2" variant="outline-danger" block @click="toggleModal">Toggle Me</b-button>
        </b-modal>
    </div>
</template>

<script>
import ListGroup from '../components/ListGroup'
import Time from '../components/Time'
import AddInput from '../components/AddInput'

export default {
    name: 'home',
    components: {
        ListGroup,
        AddInput,
        Time
    },
    data () {
        return {
            isbreakTime: false,
            todoList: [],
            todo: '',
            delItem: ''
        }
    },

    methods: {
        onAddList (value) {
            this.todoList.push({
                name: value,
                status: false,
                time: 15
            })
        },
        changeTodo (value) {
            this.todo = value
        },
        onDelTodo (value) {
            this.delItem = value
            this.$refs['my-modal'].show()

        },

        hideModal () {
            this.$refs['my-modal'].hide()
        },
        toggleModal () {
            this.$refs['my-modal'].toggle('#toggle-btn')
            let num = this.todoList.map(item => {
                return item.name
            }).indexOf(this.delItem.name)

            this.todoList.splice(num, 1)
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

