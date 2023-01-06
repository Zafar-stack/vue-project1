<template>
    <li>
        <span :class="{done: todo.completed}">
            <input type="checkbox" 
                   @change="todo.completed = !todo.completed">
            <strong>{{ index }}</strong>
            {{ todo.title }}
            <button class="rm" 
                    @click="$emit('remove-todo', todo.id)"
            >&times;</button>
        </span>
    </li>
</template>

<script>
export default {
    props : {
        todo: {
            type: Object,
            required: true
        },
        index: Number
    },
    filters: {
        upperCased: function(value) {
            return value.toUpperCase()
        }
    }
    
}
</script>

<style scoped>
    li {
        border: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: .5rem 2rem;
        margin-bottom: 1rem;
    }
    .rm{
        background-color: red;
        color: #fff;
        border-radius: 50%;
        font-weight: bold;
    }

    input{
        margin-right: 1px;
    }
    .done{
        text-decoration: line-through;
    }
</style>