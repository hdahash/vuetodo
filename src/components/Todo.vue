<template>
  <div class="todo-div">
    <ul v-for="item in datalist" :key="item.id">
      <li>{{item}}</li>
    </ul>
    
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: ["datalist"],
  data() {
    return {
      msg: "TODO App"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
