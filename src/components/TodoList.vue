<template>
  <div class="container">
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon1">待辦事項</span>
      </div>
      <input
        type="text"
        class="form-control"
        placeholder="準備要做的任務"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
      <div class="input-group-append">
        <button class="btn btn-primary" type="button" @click="addTodo">
          新增
        </button>
      </div>
    </div>
    <div class="card text-center">
      <div class="card-header">
        <ul class="nav nav-tabs card-header-tabs">
          <li class="nav-item">
            <a
              class="nav-link"
              href="#"
              :class="{ active: visibility == 'all' }"
              @click.prevent="visibility = 'all'"
              >全部
            </a>
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              href="#"
              :class="{ active: visibility == 'doing' }"
              @click.prevent="visibility = 'doing'"
              >進行中</a
            >
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              href="#"
              :class="{ active: visibility == 'done' }"
              @click.prevent="visibility = 'done'"
              >已完成</a
            >
          </li>
        </ul>
      </div>
      <ul class="list-group list-group-flush text-left">
        <li
          class="list-group-item"
          v-for="item in filteredTodos"
          :key="item.id"
          @dblclick="editTodo(item)"
        >
          <div
            class="d-flex list-group-item-block"
            v-if="item.id !== cacheTodo.id"
          >
            <div class="form-check">
              <input
                type="checkbox"
                class="form-check-input"
                :id="item.id"
                v-model="item.completed"
              />
              <label
                class="form-check-label"
                :for="item.id"
                :class="{ completed: item.completed }"
              >
                {{ item.title }}
              </label>
            </div>
            <button
              type="button"
              class="close ml-auto"
              aria-label="Close"
              @click="removeTodo(item)"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <input
            type="text"
            class="form-control"
            v-if="item.id == cacheTodo.id"
            v-model="cacheTitle"
            @keyup.esc="cancelEdit()"
            @keyup.enter="doneEdit(item)"
          />
        </li>
      </ul>
      <div class="card-footer d-flex justify-content-between">
        <span>還有 {{ uncompletedCount }} 筆任務未完成</span>
        <a href="#" @click.prevent="cleanAll">清除所有任務</a>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "TodoList",
  setup() {
    const newTodo = ref("");
    const todos = ref([]);
    const cacheTodo = ref({});
    const cacheTitle = ref("");
    const visibility = ref("all");
    const unCompletedTodos = ref([]);
    const unCompletedNum = ref("");
    function addTodo() {
      let value = newTodo.value.trim();
      if (!value) {
        return;
      }
      let timestamp = Math.floor(Date.now());
      todos.value.push({ id: timestamp, title: value, completed: false });
      newTodo.value = "";
    }
    function removeTodo(todo) {
      let newIndex = todos.value.findIndex(function (item) {
        return todo.id === item.id;
      });
      todos.value.splice(newIndex, 1);
    }
    function editTodo(item) {
      cacheTodo.value = item;
      cacheTitle.value = item.title;
    }
    function cancelEdit() {
      cacheTodo.value = {};
    }
    function doneEdit(item) {
      item.title = cacheTitle.value;
      cacheTitle.value = "";
      cacheTodo.value = {};
    }
    function cleanAll() {
      todos.value = [];
    }

    const filteredTodos = computed(() => {
      if (visibility.value === "all") {
        return todos.value;
      } else if (visibility.value === "doing") {
        let anotherTodos = [];
        todos.value.forEach(function (item) {
          if (!item.completed) {
            anotherTodos.push(item);
          }
        });
        return anotherTodos;
      } else if (visibility.value === "done") {
        let anotherTodos = [];
        todos.value.forEach(function (item) {
          if (item.completed) {
            anotherTodos.push(item);
          }
        });
        return anotherTodos;
      }
      return [];
    });
    const uncompletedCount = computed(() => {
      let unCompletedTodos = todos.value.filter(function (item) {
        return item.completed === false;
      });
      return unCompletedTodos.length;
    });
    return {
      newTodo,
      todos,
      cacheTodo,
      cacheTitle,
      visibility,
      unCompletedTodos,
      unCompletedNum,
      addTodo,
      removeTodo,
      editTodo,
      cancelEdit,
      doneEdit,
      cleanAll,
      filteredTodos,
      uncompletedCount,
    };
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
