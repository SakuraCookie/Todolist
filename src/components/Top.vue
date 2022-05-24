<template>
  <div class="todo-wrap todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车确认"
      @keyup.enter="add"
      v-model="title"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "Top",
  data() {
    return {
      title:""
    }
  },
  methods: {
    add() {
      if(!this.title.trim())
        return this.title=""
      const todoObj = { id: nanoid(), title: this.title.trim(), complete: false }
      //*通知App组件添加一个todoObj
      this.$emit("receive",todoObj)
      this.title=""
    },
  }
};
</script>

<style scoped>
/*header */
.todo-header input {
  width: calc(100% - 22px);
  height: 28px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 6px 10px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.6);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
</style>