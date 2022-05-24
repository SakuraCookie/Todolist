<template>
  <ul :class="isEmpty">
    <transition-group
      appear
      name="animate__animated animate__bounce"
      enter-active-class="animate__jackInTheBox"
      leave-active-class="animate__hinge"
    >
      <Item v-for="todo in todos" :key="todo.id" :todo="todo" />
    </transition-group>
  </ul>
</template>

<script>
import Item from "./Item.vue";
import "animate.css"

export default {
  name: "List",
  components: { Item },
  props: { todos: Array },
  computed: {
    isEmpty() {
      return this.todos.length === 0 ? "todo-empty" : "todo-main";
    },
  },
};
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0;
  border: 1px solid #ccc;
  border-radius: 2px;
  padding: 0;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ccc;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>