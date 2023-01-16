<template>
    <li>
        <span :class="{done: todo.completed}">
            <input type="checkbox" 
                   v-model="todo.completed"
                   @change="$emit('filtering-list', todo.completed)"
                   />
            <strong>{{ index + 1}}</strong>
            {{ todo.title }}    
            {{ info[0].name }}
        </span>
        <div class="btn">
            <button class="rm" @click="$emit('remove-todo', todo.id)" >&times;</button>
        </div>
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
    inject: ['info']
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
    .rmrm{
        background-color: green;
        color: #fff;
        border-radius: 50%;
        font-weight: bold;
    }

    .btn{
        display: inline-block;
    }

    button{
        margin: 0 5px 0 5px;
    }

    input{
        margin-right: 1px;
    }
    .done{
        text-decoration: line-through;
    }
</style>