<template>
  <ul id="example-1">
    <li v-for="item in items">
      {{ item.message }}
    </li>
  </ul>
</template>

<script>
export default {
  el: '#example-1',
  items: [{ message: 'Foo' }, { message: 'Bar' }],
};
</script>

<style></style>
