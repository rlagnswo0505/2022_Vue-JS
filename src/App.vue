<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @childAddTodo="addTodo" @modalShow="showModal"></TodoInput>
    <TodoList :propsItems="todoItems" @childRemoveTodo="removeTodo"></TodoList>
    <TodoFooter @childRemoveAll="removeAllTodo"></TodoFooter>
  </div>
  <AlertModal :show="modalShow" header="알림창" body="내용을 입력해 주세요." @close="closeModal"></AlertModal>
</template>

<script>
import TodoHeader from './components/todo/TodoHeader.vue';
import TodoInput from './components/todo/TodoInput.vue';
import TodoList from './components/todo/TodoList.vue';
import TodoFooter from './components/todo/TodoFooter.vue';
import AlertModal from './components/common/AlertModal.vue';
import axios from 'axios';
import DateUtils from './utils/DateUtils';

export default {
  name: 'App',
  data() {
    return {
      todoItems: [],
      cnt: 0,
      modalShow: false,
    };
  },
  methods: {
    addTodo(todoItem) {
      // if (this.todoItems.includes(todoItem)) {
      //   alert('같은 todo가 존재합니다');
      // } else {
      //   this.todoItems.push(todoItem);
      // ]
      // this.todoItems.push({
      //   key: this.cnt++,
      //   value: todoItem,
      // });
      const param = {
        todo: todoItem,
      };
      axios.post(`/todo/index`, param).then((res) => {
        if (res.status === 200 && res.data.result) {
          const item = {
            itodo: res.data,
            todo: todoItem,
            created_at: DateUtils.getTimestamp(new Date()),
          };
          this.todoItems.push(item);
        }
      });
    },
    showModal() {
      this.modalShow ? (this.modalShow = false) : (this.modalShow = true);
    },
    closeModal() {
      this.modalShow = false;
    },
    removeTodo(key) {
      this.todoItems.forEach((item, idx) => {
        if (item.itodo === key) {
          this.todoItems.splice(idx, 1);
          axios.delete(`/todo/index/${key}`).then((res) => {
            console.log(res);
          });
        }
      });
    },

    removeAllTodo() {
      this.todoItems.splice(0);
      axios.delete(`/todo/index`).then((res) => {
        console.log(res);
      });
    },
    changeValue() {
      const json = JSON.stringify(this.todoItems);
      localStorage.setItem('todoItems', json);
      localStorage.setItem('cnt', this.cnt);
    },
  },
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
    AlertModal,
  },
  // todoItems 배열 안에 값이 바뀌면 실행
  // 배열의 주솟값은 고정이라 deep으로 안에 내용이 바뀌는 지 확인해야함
  watch: {
    todoItems: {
      deep: true,
      handler() {
        this.changeValue();
      },
    },
  },
  created() {
    // const json = localStorage.getItem('todoItems');
    // if (json) {
    //   const todoItems = JSON.parse(json);
    //   todoItems.forEach((item) => {
    //     this.todoItems.push(item);
    //   });
    //   const cnt = ~~localStorage.getItem('cnt');
    //   this.cnt = cnt;
    // }
    axios.get('/todo/index').then((res) => {
      if (res.status === 200 && res.data.length > 0) {
        res.data.forEach((item) => {
          this.todoItems.push(item);
        });
      }
    });
  },
};
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f8;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
