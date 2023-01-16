<template>
  <h1>Todo app</h1>
  <AddToDo 
    @exist-todo="isExist"
    @add-todo="AddToDo" 
  />
  <p v-if="showWarning">This title is already exist!</p>
  <select v-model="filters">
    <option value="all">All</option>
    <option value="completed">Completed</option>
    <option value="not-completed">Not Completed</option>
  </select>
  <hr>
  <ToDoList 
    v-if="filteredToDo.length"
    v-bind:todos="filteredToDo"
    @filtering-list="filterList" 
    @remove-todo="removeToDo"
  />
  <p v-else>Nothing here...</p>

</template>

<script>
import ToDoList from "@/components/ToDoList.vue";
import AddToDo from "@/components/AddToDo.vue";

const StorageKey = 'todos'

export default {
  
  name: 'App',
  
  data() {
    return {
      todos: [],
      filters: 'all',
      showWarning: false
    }
  },

  computed: {
    filteredToDo() {
      if (this.filters === 'completed') {
        return this.todos.filter(function(t) {
          return t.completed
        })
      }
      if (this.filters === 'not-completed') {
        return this.todos.filter(t => !t.completed)
      }
      return this.todos
    }
  },

  created() {
    if(localStorage.getItem(StorageKey)) {
      this.todos = JSON.parse(localStorage.getItem(StorageKey))
    }
  },

  mounted() {
    // this.getData()

  },
  
  components: {
    ToDoList, AddToDo
  },

  methods: {
    
    removeToDo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
      this.saveItems()
    },

    isExist(todo) {
      return this.todos.findIndex(k=> k.title === todo.title) > -1
    },

    AddToDo(todo) {
      if (this.isExist(todo)) {
        this.showWarning = true
        return 
      }
      this.todos.push(todo)
      this.saveItems()
    },

    filterList() {
      this.todos.completed = !this.todos.completed
      this.saveItems()
    },

    saveItems() {
        let parsed = JSON.stringify(this.todos);
        localStorage.setItem(StorageKey, parsed);
    }

    // getData() {
    //   fetch('https://jsonplaceholder.typicode.com/todos?_limit=7')
    //     .then(response => response.json())
    //     .then(json => {
    //       this.todos = json
    //     })
    // }
  }


}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
