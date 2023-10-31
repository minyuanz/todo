<script setup>
import { ref, reactive, computed } from "vue";
const todoId = ref(0);
const todo = ref('');
const todoList = reactive([]);
const tabChange = ref('全部');
const tabList = reactive([{
    title: '全部'
},
{
    title: '進行中'
},
{
    title: '已完成'
}]);
const edit = ref('')



const length = computed(() => {
    let unFinish = todoList.filter(res => {
        return !res.completed
    })
    return `還有${unFinish.length}筆任務未完成`
});

const filterList = computed(() => {
    if (tabChange.value == '全部') {
        return todoList
    } else if (tabChange.value == '進行中') {
        return todoList.filter(res => {
            return !res.completed
        })
    } else if (tabChange.value == '已完成') {
        return todoList.filter(res => {
            return res.completed
        })
    }
})

const addToList = () => {
    let doing = {
        id: todoId.value,
        title: todo.value,
        completed: false,
        edit: false
    }
    todoList.push(doing)
    todoId.value++
    todo.value = ''
};
const editTitle = (todos) => {
    // console.log(todos);
    todos.edit = true
};

const editChange = (todos) => {
    // console.log(todos);
    todos.title = edit.value
    todos.edit = false
}

const deList = (id) => {
    let index = todoList.findIndex(item => item.id === id);
    if (index > -1) {
        todoList.splice(index, 1);
    }
};

const clearAll = () => {
    todoList.splice(0, todoList.length);
};
</script>

<template>
    <div class="wrap">
        <div class="title">
            <p>待辦事項</p>
            <input type="text" v-model.trim='todo' @keyup.enter="addToList">
            <button type="button" @click="addToList">新增</button>
        </div>
        <ul class="tab">
            <li v-for="tab in tabList">
                <p :class="{ 'active': tabChange == tab.title }" @click="tabChange = tab.title"> {{ tab.title }} </p>
            </li>
        </ul>
        <ul class="list">
            <li v-for="(todos, index) in filterList" :key="todos.id">
                <input type="checkbox" :id="todos.id" v-model="todos.completed">
                <input type="text" v-if="todos.edit" v-model="edit" class="edit" @keyup.enter="editChange(todos)">
                <label :for="todos.id" :class="{ 'completed': todos.completed }" v-else @dblclick="editTitle(todos)">{{
                    todos.title
                }}</label>
                <button type="button" @click="deList(todos.id)">&times;</button>
            </li>
        </ul>
        <div class="bottom">
            <div>
                <p>{{ length }}</p>
            </div>
            <div>
                <button type="button" @click="clearAll">清除所有</button>
            </div>
        </div>
    </div>
</template>

<style lang='scss' scoped>
* {
    margin: 0;
    padding: 0;
    list-style: none;
}

.active {
    background-color: #fff;
}

.completed {
    text-decoration: line-through
}

.wrap {
    width: 800px;
    margin: 100px auto;

    .title {
        display: flex;
        justify-content: center;
        // align-items: center;
        // height: 40px;
        // border: 1px solid red;

        p {
            margin: 0;
            width: 10%;
            background-color: #ddd;
            line-height: 28px;
            text-align: center;
        }

        input {
            width: 80%;
            border: 1px solid #ddd;
        }

        button {
            width: 10%;
            border: 1px solid #ddd;
        }
    }

    .tab {
        margin-top: 20px;
        display: flex;
        background-color: #eee;

        li {
            p {
                border: 1px solid transparent;
                padding: 10px 20px;
                color: #a0f;
                cursor: pointer;

                &:hover {
                    border: 1px solid #aaa;
                    border-radius: 5px 5px 0 0;
                }
            }


        }

    }

    .list {
        margin-top: 20px;

        .edit {
            margin-left: 20px;
        }

        li {
            border: 1px solid #aaa;
            padding: 10px 20px;
            display: flex;

            label {
                margin-left: 20px;
            }

            button {
                margin-left: auto;
            }
        }

        button {
            border: 1px solid #aaa;
            border-radius: 5px;
            width: 20px;
            line-height: 20px;
        }
    }

    .bottom {
        margin-top: 20px;
        padding: 10px 20px;
        border: 1px solid #aaa;
        display: flex;
        justify-content: space-between;
        align-items: center;

        button {
            border: 1px solid #aaa;
            border-radius: 5px;
            padding: 5px 10px;
        }
    }
}
</style>