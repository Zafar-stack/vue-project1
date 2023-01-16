<template>
  <h1>Todo app</h1>
  <div class="search-todo">
    <form @submit.prevent="searchToDo">
      <label for="search">Search</label>
      <input type="search" id="search" v-model="search" />
      <button type="submit">Search</button>
    </form>
  </div>
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
      showWarning: false,
      anything: [
        {name: 'qwe', age: 18},
        {name: 'asd', age: 19},
        {name: 'zxc', age: 17},
      ],
      search: ''
    }
  },

  provide() {
    return {
      info: this.anything
    }
  },
  
  components: {
    ToDoList, AddToDo
  },

  computed: {
    
    filteredToDo() {
      var tempToDo = this.todos 
      
      if(this.search.trim()) {
        tempToDo = tempToDo.filter(g => {
          return g.title.toLowerCase().includes(this.search.toLowerCase())
        })
      }
      
      if (this.filters === 'completed') {
        return tempToDo.filter(t => t.completed)
      }
      if (this.filters === 'not-completed') {
        return tempToDo.filter(t => !t.completed)
      }
      return tempToDo
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

  methods: {

    removeToDo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
      this.saveItems()
    },

    isExist(todo) {
      return this.todos.findIndex( k=> k.title === todo.title) > -1
    },

    AddToDo(todo) {
      if (this.isExist(todo)) {
        this.showWarning = true
        return 
      }
      this.todos.push(todo)
      this.saveItems()
      this.showWarning = false
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
