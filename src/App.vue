<template>
  <h1>Todo app</h1>
  <AddToDo @add-todo="AddToDo" />
  <select v-model="filter">
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
export default {
  
  name: 'App',
  
  data() {
    return {
      todos: [],
      filter: 'all',
    }
  },
  
  mounted() {
    this.getData()

    if(localStorage.getItem('items')) {
      try {
        this.items = JSON.parse(localStorage.getItem('items'));
      } catch(e) {
        localStorage.removeItem('items');
      }
    }
  },
  
  components: {
    ToDoList, AddToDo
  },

  computed: {
    filteredToDo() {
      if (this.filter === 'completed') {
        return this.todos.filter(t => t.completed)
      }
      if (this.filter === 'not-completed') {
        return this.todos.filter(t => !t.completed)
      }
      return this.todos
    }
  },

  methods: {
    removeToDo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
      this.saveItems()
    },

    AddToDo(todo) {
      // console.log(this.todos.indexOf(todo.title) = -1)
      // console.log(id)
      this.todos.push(todo)
      this.saveItems()
    },

    filterList() {
      this.todos.completed = !this.todos.completed
      this.saveItems()
    },

    getData() {
      fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
        .then(response => response.json())
        .then(json => {
          this.todos = json
        })
    },

    saveItems() {
      let parsed = JSON.stringify(this.items);
      localStorage.setItem('items', parsed);
    }
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
