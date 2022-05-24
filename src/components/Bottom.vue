<template>
  <div class="todo-footer" v-show="todos.length">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成 {{ done }}</span> / 全部 {{ todos.length }}
    </span>
    <button class="btn btn-danger" @click="clearAllTodo">
      清除已完成的任务
    </button>
  </div>
</template>

<script>
export default {
  name: "Bottom",
  computed: {
    done() {
      return this.todos.reduce((pre, e) => pre + (e.complete ? 1 : 0), 0);
    },
    isAll: {
      get() {
        return this.done === this.todos.length;
      },
      set(val) {
        this.$emit("checkAllTodo", val);
      },
    },
  },
  methods: {
    clearAllTodo() {
      this.$emit("clearAllTodo")
    }
  },
  props: { todos: Array },
};
</script>

<style scoped>
/*footer */
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding: 4px 6px;
  margin-top: 5px;
  user-select: none;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>