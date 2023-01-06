<template>
    <div>
        <ul>
            <ToDoItem 
                v-for="(todo, i) in todos" 
                :todo="todo"
                :index="i"
                @remove-todo="removeToDo"
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
        }
    }
}

</script>

<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }
</style>