<script setup>
import { ref, computed, reactive } from "vue";
const tabList = reactive([{
    name: '全部',
    value: 'all'
}, {
    name: '進行中',
    value: 'active'
}, {
    name: '已完成',
    value: 'completed'
},]);
const visibility = ref('all')
const todo = ref('');
const todolist = ref([]);
const editText = ref('')

const filterList = computed(() => {
    if (visibility.value == "all") {
        return todolist.value;
    } else if (visibility.value == "active") {
        return todolist.value.filter((item) => {
            return !item.completed;
        });
    } else {
        return todolist.value.filter((item) => {
            return item.completed;
        });
    }
});

const todoLength = computed(() => {
    let lengthlist = todolist.value.filter((item) => {
        return !item.completed;
    }).length
    return lengthlist;
})

const addtolist = () => {
    if (!todo.value) {
        return
    }
    let todoitem = {
        title: todo.value,
        completed: false,
        edit: false
    }
    todolist.value.push(todoitem)
    todo.value = ''
};

const deltolist = (index) => {
    todolist.value.splice(index, 1)
};

const cleartolist = () => {
    todolist.value = []
};

const edit = (item) => {
    console.log(item);
    item.edit = true
};

const editTitle = (item) => {
    console.log(item);
    item.title = editText.value
    item.edit = false
};
</script>

<template>
    <div id="app" class="container my-5">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <span class="input-group-text" id="basic-addon1">待辦事項</span>
            </div>
            <input type="text" class="form-control" placeholder="準備要做的任務" v-model.trim="todo" @keyup.enter="addtolist">
            <div class="input-group-append">
                <button class="btn btn-primary" type="button" @click="addtolist">新增</button>
            </div>
        </div>
        <div class="card text-center">
            <div class="card-header">
                <ul class="nav nav-tabs card-header-tabs">
                    <li class="nav-item" v-for="tab in tabList">
                        <a class="nav-link " href="#" :class="{ 'active': visibility == tab.value }"
                            @click="visibility = tab.value">{{ tab.name }}</a>
                    </li>
                </ul>
            </div>
            <ul class="list-group list-group-flush text-left">
                <li class="list-group-item" v-for="(item, index) in filterList" @dblclick="edit(item)">
                    <div class="d-flex">
                        <div class="form-check">
                            <input type="checkbox" class="form-check-input" v-model="item.completed">
                            <input type="text" class="form-control" v-if="item.edit" v-model="editText"
                                @keyup.enter="editTitle(item)">
                            <label class="form-check-label" :class="{ 'completed': item.completed }" v-else>
                                {{ item.title }}
                            </label>

                        </div>
                        <button type="button" class="close ml-auto del " aria-label="Close" @click="deltolist(index)">
                            <span>&times;</span>
                        </button>
                    </div>
                </li>
                <li class="list-group-item">
                    <input type="text" class="form-control">
                </li>
            </ul>
            <div class="card-footer d-flex justify-content-between">
                <span>還有 {{ todoLength }} 筆任務未完成</span>
                <a href="#" @click="cleartolist">清除所有任務</a>
            </div>
        </div>
    </div>
</template>

<style lang='scss' >
* {
    margin: 0;
    padding: 0;
    list-style: none;
}

#app {
    width: 800px;
    margin: auto;

    .del {
        width: 30px;
        height: 30px;
        margin-left: auto;
        border: 1px solid transparent;
    }

    .completed {
        text-decoration: line-through
    }
}
</style>