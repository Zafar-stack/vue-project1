<template>
    <div class="show-todo">
        <li>
            <span :class="{done: todo.completed}">
                <input type="checkbox" 
                       class="checkbox" 
                       v-model="todo.completed" 
                       @change="$emit('filtering-list', todo.completed)"
                       />
                <strong class="strn">{{ index + 1 }}</strong>
                {{ todo.title }}    
                <!-- {{ info[0].name }} -->
            </span>
            <div class="btns">
                <button class="btn1" @click="$emit('edit-list', todo)" >🖋</button>
                <button class="btn" @click="$emit('remove-todo', todo.id)" >❌</button>
            </div>
        </li>
    </div>
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
    // inject: ['info']
}
</script>

<style scoped>
    .show-todo{
        text-align: center;
    }

    .show-todo li {
        display: flex;
        justify-content: space-between;
        padding: 5px;
        margin: 10px;
        border: 1px solid #008080;
        border-radius: 4px;
        text-align: center;
    }
    .show-todo span{
        padding: 5px;
    }

    .btn{
        padding: 5px;
        background: none;
        border-radius: 100%;
        color: paleturquoise;
        background-color: red;
        align-items: center;
        margin-left: 5px;
    }
    .btn1{
        padding: 5px;
        background: none;
        border-radius: 100%;
        color: paleturquoise;
        background-color: grey;
        align-items: center;
        margin-left: 5px;
    }

    .done{
        text-decoration: line-through;
    }

    .strn{
        padding: 5px;
        align-items: center;
    }
    

</style>