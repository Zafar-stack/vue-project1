<template>
  <div class="app">
    
    
    <h1>Todo app</h1>
    <div class="main">
      <div class="search-todo">
          <select v-model="filters">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not Completed</option>
          </select>
          <form @submit.prevent="filteredToDo">
            <input 
              type="search" 
              id="search" 
              v-model="search" 
              placeholder="Search here..." 
            />
            <button 
              type="submit"
            >Search</button>
          </form>
      </div>
    
    
      <AddToDo 
        @exist-todo="isExist"
        @add-todo="AddToDo" 
        :editingData="editingData"
      />
    
      
      
      
      <p v-if="showWarning">This title is already exist!</p>
      
      
      <div class="container">
        <ToDoList 
          v-if="filteredToDo.length" 
          v-bind:todos="paginate(filteredToDo)" 
          @filtering-list="filterList" 
          @remove-todo="removeToDo"  
          @edit-list="editList" 
        />
        
        <p v-else>Nothing here...</p>
        
        
        
        <div class="pagination-nav">
          <button 
            class="pagination-pn" 
            type="button" 
            v-if="page != 1" 
            @click="page--" 
            > &lt&lt 
          </button>
          <button 
            class="pagination-circle" 
            type="button" 
            v-for="pageNumber in setPages" 
            :class="{active: isPageActive(pageNumber)}" 
            @click="page = pageNumber"
          > {{pageNumber}} 
          </button>
          <button 
            class="pagination-pn" 
            type="button" 
            @click="page++" 
            v-if="page < setPages" 
          > >> 
          </button>
        </div>
      </div>


    </div>  
  </div>
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
      // anything: [
      //   {name: 'qwe', age: 18},
      //   {name: 'asd', age: 19},
      //   {name: 'zxc', age: 17},
      // ],
      search: '',
      page: 1,
      limit: 5,
      editingData: {}
    }
  },

  // provide() {
  //   return {
  //     info: this.anything
  //   }
  // },
  
  components: {
    ToDoList, AddToDo
  },

  computed: {
    
    filteredToDo() {
      var tempToDo = this.todos
      this.showWarning = false

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
      if (this.filters === 'all') {
        return tempToDo
      }
      
    },

    setPages() {
      return Math.ceil(this.filteredToDo.length / this.limit);
    },
  },

  created() {
    this.getToDo()
  },


  mounted() {
    // this.getData()

  },

  methods: {

    isPageActive(page) {
      return this.page === page;
    },

    getToDo() {
      if(localStorage.getItem(StorageKey)) {
        this.todos = JSON.parse(localStorage.getItem(StorageKey))
      }           
    },

    paginate (todos) {
      let page = this.page;
      let limit = this.limit;
      let from = (page * limit) - limit;
      let to = (page * limit);
      return todos.slice(from, to);
    },

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

    editList(todo) {
      this.editingData = todo
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
  @import './assets/style.css';
</style>