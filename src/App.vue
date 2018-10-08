<template>
  <div id="app">
    <img width="25%" src="./assets/logo.png">
    <Todo :datalist="list"/>
    <p v-if="newItemName">{{ newItemName }}  {{ newItemDueDate }}</p>
    <label>Task Name</label><input v-model="newItemName">
  <label>Due Date</label><input v-model="newItemDueDate">
  <button v-on:click="addNew">Add</button>
  </div>
</template>

<script>
import Todo from "./components/Todo";
import Dexie from "dexie";

var db = new Dexie("Todo");

db.version(1).stores({
  task: "++id,name,duedate"
});

db.open();

export default {
  name: "App",
  components: {
    Todo
  },
  data() {
    return {
      list: [],
            newItemName:'',
	newItemDueDate:''
    };
  },
  beforeMount: function() {
    //console.log("before mount");
    var vueapp = this;
    db.task.toArray().then(function() {
      return db.task.each(task =>
        vueapp.list.push([task.id, task.name, task.duedate, 0])
      );
    });
    //console.log(arr);
    //this.list =arr;
    //console.log(this.list);
    //this.list=db.task.toArray();
  },
  methods: {
    taskMouseover: function(item) {
      console.log('mouse over');
      var index = this.list.indexOf(item);
      var temparr = this.list[index];
      temparr[3] = 1;
      this.$set(this.list, index, temparr);
    },
    taskMouseout: function(item) {
      console.log('mouse out');
      var index = this.list.indexOf(item);
      var temparr = this.list[index];
      temparr[3] = 0;
      this.$set(this.list, index, temparr);
    },
    addNew: function() {
      //this.list.push(this.newItem);
      var vapp = this;
      db.task
        .put({
          name: vapp.newItemName,
          duedate: vapp.newItemDueDate /*new Date().toLocaleDateString()*/
        })
        .then(function(id) {
          vapp.list.push([id, vapp.newItemName, vapp.newItemDueDate, 0]);
          vapp.newItemName = "";
          vapp.newItemDueDate = "";
        });
    },
    deleteItem: function(item) {
      var index = this.list.indexOf(item);
      var vueapp = this.vueapp;
      db.task
        .where("id")
        .equals(item[0])
        .delete()
        .then(function(deleteCount) {
          console.log("Deleted " + deleteCount + " objects");
          vueapp.list.splice(index, 1);
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
