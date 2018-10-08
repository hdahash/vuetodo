<template>
  <div class="todo-div">
    <ul >
      <li v-for="item in datalist" :key="item.id">
        <span :id="'item'+item[0]" v-on:mouseover="taskMouseover(item)" v-on:mouseout="taskMouseout(item)"> {{item[1]}} {{item[2]}}
        <a v-show="item[3]==1" class="button is-danger is-outlined is-rounded is-small" v-on:click="deleteItem(item)">
          <span class="icon is-small">
            <i class="fas fa-times"></i>
          </span>
        </a>
		
		<a v-show="item[3]==1" class="button is-primary is-outlined is-rounded is-small" v-on:click="editItem(item)">
          <span class="icon is-small">
            <i class="fas fa-edit"></i>
          </span>
        </a>
        </span>
      </li>
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
  },
  methods:{
    taskMouseover: function(item) {
      //console.log('mouse over');
      var index = this.datalist.indexOf(item);
      var temparr = this.datalist[index];
      temparr[3] = 1;
      this.$set(this.datalist, index, temparr);
    },
    taskMouseout: function(item) {
      //console.log('mouse out');
      var index = this.datalist.indexOf(item);
      var temparr = this.datalist[index];
      temparr[3] = 0;
      this.$set(this.datalist, index, temparr);
    },
  },
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
