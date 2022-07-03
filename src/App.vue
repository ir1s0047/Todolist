<template>
  <div>
    <!-- 除了驼峰, 还可以使用-转换链接 -->
    <TodoHeader @add="addFn"></TodoHeader>
    <TodoMain :list="showlist" @del="delFn"></TodoMain>
    <TodoFooter
      :count="count"
      @filterdata="filterdataFn"
      @clear="clearFn"
    ></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoMain from './components/TodoMain';
import TodoFooter from './components/TodoFooter';

export default {
  data() {
    // 1、铺设数据
    // App.vue =>main.vue 父传递子
    // 子组件 用props 去接一下传递的数据
    // 子组件 渲染数据 v-for
    // 父组件 传递
    return {
      list: JSON.parse(localStorage.getItem('todoList')) || [],
      getSel: 'all',
    };
  },
  methods: {
    addFn(val) {
      const id = this.list[this.length - 1]
        ? this.list[this.length - 1].id + 1
        : 100;
      this.list.push({
        id,
        name: val,
        isDone: false,
      });
    },
    delFn(id) {
      const index = this.list.findIndex((ele) => ele.id == id);
      this.list.splice(index, 1);
    },
    filterdataFn(val) {
      this.getSel = val;
    },
    clearFn() {
      this.list.forEach((ele) => (ele.isDone = false));
    },
  },
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  computed: {
    count() {
      return this.list.filter((ele) => !ele.isDone).length;
    },
    showlist() {
      if (this.getSel == 'no') {
        return this.list.filter((ele) => !ele.isDone);
      } else if (this.getSel == 'yes') {
        return this.list.filter((ele) => ele.isDone);
      } else {
        return this.list;
      }
    },
  },
  watch: {
    list: {
      deep: true,
      handler() {
        // 8.0 只要list变化 - 覆盖式保存到localStorage里
        localStorage.setItem('todoList', JSON.stringify(this.list));
      },
    },
  },
};
</script>
