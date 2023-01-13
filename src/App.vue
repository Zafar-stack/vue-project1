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
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(response => response.json())
      .then(json => {
        this.todos = json
      })
  },
  components: {
    ToDoList, AddToDo
  },
  computed: {
    filteredToDo() {
      if (this.filter === 'all') {
        return this.todos
      }

      if (this.filter === 'completed') {
        return this.todos.filter(t => t.completed)
      }

      if (this.filter === 'not-completed') {
        return this.todos.filter(t => !t.completed)
      }
    }
  },
  methods: {
    removeToDo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    },
    AddToDo(todo) {
      this.todos.push(todo)
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
  max-width: 21px;
}
</style>
