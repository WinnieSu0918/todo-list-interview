<template>
  <TodoList />
</template>

<script>
import TodoList from "@/components/TodoList.vue";
export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.completed {
  text-decoration: line-through;
}

.list-group-item-block {
  display: flex;
  justify-content: space-between;
}

.close {
  background: transparent;
  border: none;
}
</style>
