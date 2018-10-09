<template>
  <div id="app">
    <Todo :datalist="list"/>
    <p v-if="newItemName">{{ newItemName }}  {{ newItemDueDate }}</p>
    <label>Task Name</label><input v-model="newItemName">
  <label>Due Date</label><input v-model="newItemDueDate">
  <button v-show="!editMode" v-on:click="addNew">Add</button>
  <button v-show="editMode" v-on:click="saveItem">Save</button>
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
	newItemDueDate:'',
  currentItem:-1,
  editMode:false
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
    saveItem: function(){
      //console.log(this.currentItem);
      var vueapp = this;
      var temparr = this.list[this.currentItem];
      temparr[1] = this.newItemName;
      temparr[2] = this.newItemDueDate;
      db.task.update(temparr[0], {name: this.newItemName, duedate:this.newItemDueDate}).then(function (updated) {
  if (updated)
    {
      vueapp.$set(vueapp.list, vueapp.currentItem, temparr);
      vueapp.newItemName = "";
      vueapp.newItemDueDate = "";
    }
  else
   console.log ("Nothing was updated - there were no friend with primary key: 2");
});
    this.editMode = false;  
    },
    deleteItem: function(item) {
      var index = this.list.indexOf(item);
      var vueapp = this;
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
<style scoped>
p {
color:#ffffff;
background-color:#008080
}
</style>

