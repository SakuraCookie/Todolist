<template>
  <div id="root">
    <div class="todo-container">
      <Top @receive="addTodo" />
      <List :todos="todos" />
      <Bottom
        :todos="todos"
        @checkAllTodo="checkAllTodo"
        @clearAllTodo="clearAllTodo"
      />
    </div>
  </div>
</template>

<script>
import Header from "./components/Top.vue";
import List from "./components/List.vue";
import Top from "./components/Top.vue";
import Bottom from "./components/Bottom.vue";

export default {
  name: "App",
  components: { Header, List, Top, Bottom },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    //?添加一个todo
    addTodo(x) {
      this.todos.unshift(x);
    },
    //?勾选/取消勾选一个todo
    checkTodo(x) {
      this.todos.forEach((e) => {
        if (e.id === x) e.complete = !e.complete;
      });
    },
    //?删除一个指定todo
    deleteTodo(x) {
      this.todos = this.todos.filter((e) => e.id != x);
    },
    //?全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((e) => (e.complete = done));
    },
    //?清除所有已完成的todo
    clearAllTodo() {
      this.todos = this.todos.filter((e) => !e.complete);
    },
    //?更新指定todo的title
    updateTodo(id, val) {
      this.todos.forEach((e) => {
        if (e.id === id) e.title = val;
      });
    },
  },
  watch: {
    todos: {
      deep: true,
      handler(newVal) {
        localStorage.setItem("todos", JSON.stringify(newVal));
      },
    },
  },
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("updateTodo", this.updateTodo);
  },
  beforeDestroy() {
    this.$bus.$off(["deleteTodo", "checkTodo", "updateTodo"]);
  },
};
</script>

<style>
/*base */
:root {
  animation-duration: 0.5s;
}
body {
  background-color: #000;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 9px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd352f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd352f;
}

.btn-success {
  color: #fff;
  background-color: #45864b;
  border: 1px solid #247555;
}

.btn-success:hover {
  color: #fff;
  background-color: #247555;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
  padding: 15px;
  border: 1px solid #ddd;
  background-color: #fff;
}

.todo-container .todo-wrap {
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>