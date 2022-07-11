<template>
  <h1>헬로우</h1>
  <div>{{ hello }}</div>
  <div>{{ hello }}</div>
  <div>{{ hello }}</div>
  <div>{{ hello }}</div>
  <input type="text" v-model="hello" name="" id="" />
  <button @click="changeHello">안녕하세요 변경</button>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      hello: '안녕',
    };
  },
  methods: {
    changeHello() {
      this.hello = '안녕하세요';
    },
  },
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  text-align: center;
  margin-top: 60px;
}
</style>
