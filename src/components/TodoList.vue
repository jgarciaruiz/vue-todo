<template>
  <div class="todo-list">
    <div class="addbox-wr">
      <input type="text" name="addbox" v-model.trim="addBox" @keyup.enter="add()">
      <button @click="add()">Add Task</button>
    </div>
    <div v-if="todoList.length">
      <div v-if="incompleted.length" class="added-tasks">
        <h3>Pending tasks ({{incompleted.length}})</h3>
        <ul>
          <li v-for="(todo, index) in todoList" :key="index" :class="{hide:todo.completed == true}">
            <p>
              <input type="checkbox" @change="complete(todo)">{{ todo.item }} <button @click="remove(todo)" :disabled="todo.completed == true">Delete</button>
              </p>
          </li>
        </ul>
      </div>
      <div v-else>
        <p>All tasks completed!</p>
      </div>
      <div v-if="completed.length" class="completed-tasks">
        <div class="added-tasks">
          <h3>Completed tasks ({{completed.length}})</h3>
          <ul>
            <li v-for="(todo, index) in completed" :key="index" :class="{done:todo.completed == true}">
              <p>{{ todo.item }} <button @click="undo(todo)">Undo</button></p>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div v-else>
      <p>There are no tasks yet</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      todoList: [],
      addBox: ''
    }
  },
  computed: {
    completed: function () {
      return this.todoList.filter(todo => todo.completed)
    },
    incompleted: function () {
      return this.todoList.filter(todo => !todo.completed)
    }
  },  
  methods: {
    add: function(todo) {
      todo = this.addBox;
      this.todoList.push({item: todo, completed: false});
      this.addBox = '';
    },
    remove: function(todo) {
      var index = this.todoList.indexOf(todo);
      this.todoList.splice(index, 1);
    },
    complete: function(todo){
      todo.completed = !todo.completed;
    },
    undo: function(todo){
      todo.completed = !todo.completed;
    }
  }
}
</script>

<style>
.hide{
  display:none;
}
.todo-list {
  width: 600px;
  margin: 0 auto;
}
ul {
  list-style-type: none;
  padding: 0;
  text-align: left;
}
li {
  display: block;
  margin: 0 10px;
}
.done {
	color: #d9d9d9;
	text-decoration: line-through;
}

</style>
