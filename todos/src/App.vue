<template>
  <div>
    <todo-input @add="onAddNewTask"></todo-input>
    <todo-list :list="tasklist" class="mt-2"></todo-list>
    <todo-button v-model:active="activeBtnIndex"></todo-button>
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";
import TodoInput from "./components/TodoInput.vue";
import TodoButton from "./components/TodoButton.vue";
export default {
  name: "MyApp",
  data() {
    return {
      todolist: [
        { id: 1, task: "周一早晨9点开会", done: false },
        { id: 2, task: "周一晚上9点聚餐", done: false },
        { id: 3, task: "准备周三上午的演讲稿", done: true },
      ],
      nextId: 4,
      activeBtnIndex: 0,
    };
  },
  computed: {
    tasklist() {
      switch (this.activeBtnIndex) {
        case 0:
          return this.todolist;
        case 1:
          return this.todolist.filter((x) => x.done === true);
        case 2:
          return this.todolist.filter((x) => x.done !== true);
      }
    },
  },
  methods: {
    onAddNewTask(taskname) {
      this.todolist.push({
        id: this.nextId,
        task: taskname,
        done: false,
      });

      this.nextId++;
    },
  },
  components: {
    TodoList,
    TodoInput,
    TodoButton,
  },
};
</script>

<style lang="less" scoped></style>
