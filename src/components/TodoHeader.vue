<template>
  <header class="header">
    <h1>todos</h1>
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll" />
    <label for="toggle-all"></label>
    <!-- label 可以关联一个表单标签 -->
    <input
      class="new-todo"
      placeholder="输入任务名称-回车确认"
      autofocus
      @keydown.enter="enter"
      v-model.trim="task"
    />
  </header>
</template>

<script>
export default {
  data() {
    return {
      task: '',
    };
  },
  methods: {
    enter() {
      if (this.task.length == 0) return alert('任务名不能为空');
      this.$emit('add', this.task);
      this.task = '';
    },
  },

  computed: {
    isAll: {
      set(checked) {
        this.$parent.list.forEach((ele) => (ele.isDone = checked));
      },
      get() {
        //取值
        //list 里边的值
        //this.$parent 父组件
        return this.$parent.list.every((ele) => ele.isDone);
      },
    },
  },
};
</script>
