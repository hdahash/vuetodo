<template>
  <div class="todo">
<ul>
      <li v-for="item in list">
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
      
      <li v-if="newItemName">{{ newItemName }}  {{ newItemDueDate }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  props: ["list"],
  data() {
    return {
      msg: "Welcome to ToDo App",
      isBtnVisible: false,
      newItemName: "",
      newItemDueDate: ""
    };
  }
};
</script>
