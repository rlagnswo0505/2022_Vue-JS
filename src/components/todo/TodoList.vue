<template>
  <section>
    <ul>
      <li :key="todoItem" v-for="(todoItem, index) in todoItems" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        {{ todoItem }}
        <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)"><i class="far fa-trash-alt" aria-hidden="ture"></i></span>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      todoItems: [],
    };
  },
  methods: {
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    },
  },
  created() {
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {
        this.todoItems.push(localStorage.key(i));
      }
    }
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  padding-left: 0%;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background-color: #fff;
  border-radius: 5px;
}
.checkBtn {
  line-height: 50px;
  color: #62acde;
  margin-right: 5px;
}
.removeBtn {
  cursor: pointer;
  margin-left: auto;
  color: #de4343;
}
</style>
