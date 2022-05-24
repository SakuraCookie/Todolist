<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.complete"
        @change="handleCheck(todo.id)"
      />

      <!--!不推荐-->
      <!-- 
        <input type="checkbox" v-model="todo.complete">
       -->
      <span v-text="todo.title" v-show="!todo.isEdit"></span>
      <input
        type="text"
        :value="todo.title"
        v-show="todo.isEdit"
        @keydown.enter="handleEnter($event, todo)"
        @blur="handleBlur($event, todo)"
        ref="text"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      class="btn btn-success"
      @click="handleEdit(todo)"
      v-show="!todo.isEdit"
    >
      编辑
    </button>
  </li>
</template>

<script>
export default {
  name: "Item",
  //*声明接收外部参数传入
  props: { todo: Object },
  methods: {
    handleCheck(id) {
      //*通知App组件选中对应的id 改变选择状态
      this.$bus.$emit("checkTodo", id);
    },
    handleDelete(id) {
      if (confirm("确定删除"))
        //*通知App组件删除对应的id
        this.$bus.$emit("deleteTodo", id);
    },
    handleEdit(todo) {
      todo.hasOwnProperty("isEdit")
        ? (todo.isEdit = true)
        : this.$set(todo, "isEdit", true);
      //*也可以用setTimeout 来设置获得焦点
      //*$nextTick中的回调会在下一次DOM更新完毕之后才执行
      this.$nextTick(() => this.$refs.text.focus());
    },
    handleEnter(e, todo) {
      this.handleBlur(e, todo);
    },
    handleBlur(e, todo) {
      if (!e.target.value.trim()) return;
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
      todo.isEdit = false;
    },
  },
};
</script>

<style scoped>
/*Item */
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ccc;
}

li label {
  float: left;
  user-select: none;
  cursor: pointer;
}

li label input[type="checkbox"] {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
  margin-left: 8px;
}

li:hover button {
  display: block;
}

li::before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>