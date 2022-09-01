<script setup>
import { ref, reactive, onMounted } from "vue";
const inputContent = ref("");
let todoList = reactive([]);

onMounted(() => {
  console.log(2323)
  initTodoList();
});

const initTodoList = () => {
  const _todoList = localStorage.getItem('todoList')
  if(_todoList) {
    todoList = reactive(JSON.parse(_todoList))
  }
};

const enterHandler = () => {
  if (inputContent.value) {
    const content = inputContent.value.trim();
    todoList.push({
      content,
      done: ref(false),
      key: `${inputContent.value}${Date.now()}`,
    });
    inputContent.value = "";
    localStorage.setItem('todoList', JSON.stringify(todoList))
  }
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
        <input type="checkbox" v-model="todo.done" />
        <span>{{ index + 1 }}. {{ todo.content }}</span>
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
    }
  }
}
</style>
