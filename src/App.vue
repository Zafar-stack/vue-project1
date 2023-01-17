<template>
  <div class="app">
    <h1>Todo app</h1>
    <div class="search-todo">
      <form @submit.prevent="searchToDo">
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
    />
    <p v-if="showWarning">This title is already exist!</p>
    <div class="filters">
      <select v-model="filters" class="filter-todo">
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="not-completed">Not Completed</option>
      </select>
    </div>
    <ToDoList 
      v-if="filteredToDo.length"
      v-bind:todos="filteredToDo"
      @filtering-list="filterList" 
      @remove-todo="removeToDo"
    />
    <p v-else>Nothing here...</p>
    <div>
      <button 
        class="pagination-pn" 
        type="button" 
        v-if="page != 1" 
        @click="page--"
        > &lt&lt </button>
      <button
        class="pagination-circle" 
        type="button" 
        v-for="pageNumber in pages.slice(page-1, page+5)" 
        :class="{active: isPageActive(pageNumber)}" 
        @click="page = pageNumber"
      > {{pageNumber}} </button>
      <button 
        class="pagination-pn" 
        type="button" 
        @click="page++" 
        v-if="page < pages.length" 
      > >> </button>
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
      perPage: 5,
      pages: [],
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
      
      if(this.pages) {
        return this.paginate(tempToDo);
      }

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
    this.getToDo()
  },

  watch: {
    todos () {
      this.setPages();
    }
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

    setPages() {
      let numberOfPages = Math.ceil(this.todos.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },

    paginate (todos) {
      let page = this.page;
      let perPage = this.perPage;
      let from = (page * perPage) - perPage;
      let to = (page * perPage);
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
* {
  margin: 0;
  padding: 0;
}

.app{
  padding: 20px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}

.search-todo{
  padding: 15px;
  text-align: left;       
}
.search-todo input{
  border: 1px solid #008080;
  padding: 10px 15px;
  border-radius: 4px;
}
.search-todo button{
  padding: 10px 15px;
  background: none;
  color: darkslategray;
  border: 1px solid black; 
  border-radius: 4px;
  margin-left: 5px;  
}

.filters{
  padding: 15px;
}
.filter-todo{
  border: 1px solid #008080;
  padding: 10px 15px;
  border-radius: 4px;
}

.pagination-circle{
  padding: 10px 15px;
  background: none;
  color: darkslategray;
  border: 1px solid black; 
  border-radius: 50%;
  margin: 5px;  
}

.pagination-pn{
  padding: 10px 15px;
  background: none;
  color: darkslategray;
  border: 1px solid black; 
  border-radius: 30%;
  margin: 5px;  
}

.active {
  background-color: #4AAE9B;
  color: #ffffff;
}

</style>
