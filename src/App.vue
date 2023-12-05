<template>
  <TodoHeader />
  <TodoInput @add="addTodoItem" />
  <TodoList :todoItems="todoItems" />
</template>

<script>
import { ref } from "vue";
import TodoHeader from "./components/TodoHeader.vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
  },
  setup() {
    // data
    const todoItems = ref([]);

    // methods
    function fetchTodos() {
      const result = [];
      for (let i = 0; i < localStorage.length; i++) {
        const todoItem = localStorage.key(i);
        result.push(todoItem);
      }
      return result;
    }
    todoItems.value = fetchTodos();

    function addTodoItem(todo) {
      todoItems.value.push(todo);
      localStorage.setItem(todo, todo);
    }

    return { todoItems, addTodoItem };
  },
};
</script>
