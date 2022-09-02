<script setup>
import { ref, reactive, onMounted } from "vue";
import vFocus from "../directives/vFocus";
const inputContent = ref("");
let todoList = reactive([]);

onMounted(() => {
  console.log(2323);
  // initTodoList();
});

// const initTodoList = () => {
//   const _todoList = localStorage.getItem('todoList')
//   if(_todoList) {
//     todoList = reactive(JSON.parse(_todoList))
//   } else {
//     todoList = reactive([])
//   }
// };

const enterHandler = () => {
  if (inputContent.value) {
    const content = inputContent.value.trim();
    todoList.push({
      content,
      done: ref(false),
      key: `${inputContent.value}${Date.now()}`,
      isEdit: ref(false),
    });
    inputContent.value = "";
    localStorage.setItem("todoList", JSON.stringify(todoList));
  }
};

const editTodo = (index) => {
  console.log(index);
  todoList[index].isEdit = true;
};

const finishEdit = (index) => {
  todoList[index].isEdit = false;
};

const deleteTodo = (index) => {
  todoList.splice(index, 1);
};
</script>

<template>
  <div class="container">
    <h3>todoList</h3>
    <input
      type="text"
      placeholder="请输入"
      v-model="inputContent"
      @keyup.enter="enterHandler"
    />
    <div class="todo-container">
      <div class="todo" v-for="(todo, index) in todoList" :key="todo.key">
        <span>
          <input type="checkbox" v-model="todo.done" />
          <span v-if="!todo.isEdit" @dblclick="editTodo(index)" class="content"
            >{{ index + 1 }}. {{ todo.content }}</span
          >
          <input
            v-focus
            type="text"
            v-else
            v-model="todo.content"
            @blur="finishEdit(index)"
          />
        </span>
        <button @click="deleteTodo(index)">删除</button>
      </div>
    </div>
  </div>
</template>

<style lang="less">
.container {
  width: 400px;
  margin: 0 auto;
  > input {
    width: 100%;
  }
  .todo-container {
    width: 100%;
    margin-top: 12px;

    .todo {
      display: flex;
      justify-content: space-between;
      margin-bottom: 8px;

      .content {
        cursor: pointer;
      }
    }
  }
}
</style>
