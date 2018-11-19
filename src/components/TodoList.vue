<template>
  <div class="todo-list">
    <div class="addbox-wr">
      <input type="text" name="addbox" v-model.trim="addBox" v-on:keyup.enter="addTodo">
      <button @click="addTodo()">Add Task</button>
    </div>
    <div v-if="todoList.length">
      <div v-if="incompletedTasks.length" class="added-tasks">
        <h3>Pending tasks ({{incompletedTasks.length}})</h3>
        <ul>
          <li v-for="(todo, index) in todoList" :key="index">
            <p>
              <input type="checkbox" v-on:change="completeTask(todo)">{{index+1}}. {{ todo.item }} <button @click="deleteTodo(todo)">Delete</button>
              </p>
          </li>
        </ul>
      </div>
      <div v-else>
        <p>All tasks completed!</p>
      </div>
      <div v-if="completedTasks.length" class="completed-tasks">
        <div class="added-tasks">
          <h3>Completed tasks ({{completedTasks.length}} of {{incompletedTasks}})</h3>
          <ul>
            <li v-for="(todo, index) in todoList" :key="index">
              <p>{{index+1}}. {{ todo.item }} <button @click="undoComplete(todo)">Undo</button></p>
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
  name: 'todoList',
  props: {
    
  },

  computed: {
    completedTasks: function () {
      return this.todoList.filter(function(item) { 
        return item.completed
      })
    },
    incompletedTasks: function () {
      return this.todoList.filter(function(item) { 
        return !item.completed
      })
    }
  },  

  methods: {
    addTodo: function(todo) {
      todo = this.addBox;
      this.todoList.push({item: todo, completed: false});
      this.addBox = '';
    },
    deleteTodo: function(todo) {
      var index = this.todoList.indexOf(todo);
      this.todoList.splice(index, 1);
    },
    completeTask: function(todo){
      todo.completed = !todo.completed;
    },
    undoComplete: function(todo){
      todo.completed = !todo.completed;
    }
  },
  
  data () {
    return {
      todoList: [],
      addBox: ''
    }
  }
}
</script>

<style>
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
