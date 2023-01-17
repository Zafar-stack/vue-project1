<template>
    <div class="todolist">
        <ul>
            <ToDoItem 
                v-for="(todo, i) in todos" 
                :todo="todo" 
                :index="i" 
                :key="todo.id" 
                @remove-todo="removeToDo" 
                @filtering-list="filterList"
            />
        </ul>
    </div>
</template>

<script>
import ToDoItem from '@/components/ToDoItem.vue'
export default {
    props: ['todos'],
    components: {
        ToDoItem
    },
    methods: {
        removeToDo(id) {
            this.$emit('remove-todo', id)
        },
        filterList(completed) {
            this.$emit('filtering-list', completed)
        },
    }
}

</script>

<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }

    .todolist{
        padding: 20px;
    }
</style>