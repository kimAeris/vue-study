<template>
  <TodoHeader :appTitle="title" />
  <TodoInput @add="addTodoItem" />
  <TodoList :todoItems="todoItems" @remove="removeTodoItem" />
</template>

<script>
import { onBeforeMount, onMounted, onUnmounted, ref } from "vue";
import TodoHeader from "./components/TodoHeader.vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
  },
  data() {
    return {
      title: "Todo App",
    };
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

    console.log("1 : setup called");

    // 라이프 사이클 API가 적용되는 구간
    onBeforeMount(() => {
      console.log("2: onBeforeMount called");
      todoItems.value = fetchTodos();
    });

    onMounted(() => {
      console.log("3");
    });

    onUnmounted(() => {
      console.log(
        "4 : 컴포넌트가 사라졌을 때 호출 (페이지 떠남 or v-if를 통한 컴포넌트 제거)"
      );
    });

    function addTodoItem(todo) {
      todoItems.value.push(todo);
      localStorage.setItem(todo, todo);
    }

    return { todoItems, addTodoItem };
  },
  methods: {
    removeTodoItem(item, index) {
      this.todoItems.splice(index, 1); // setup 안에 있는 데이터에 접근 가능
      localStorage.removeItem(item);
    },
    // setup 은 원하는 구간에서 사용 가능하다 !
  },
};
</script>
