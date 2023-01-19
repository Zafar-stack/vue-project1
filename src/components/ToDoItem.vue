<template>
    <div class="show-todo">
        <li>
            <span :class="{done: todo.completed}">
                <label for="{{todo.id}}">
                    <input type="checkbox" 
                        v-model="todo.completed" 
                        @change="$emit('filtering-list', todo.completed)"
                        />
                    <strong class="strn"> {{ index + 1 }}</strong>
                    <p>{{ todo.title }}</p>    
                </label>
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
        align-items: center;
    }
    .show-todo span{
        padding: 5px;
    }

    .show-todo li span label{
        display: flex;
    }

    .btn{
        padding: 5px;
        background: none;
        border-radius: 100%;
        color: paleturquoise;
        background-color: red;
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

    .strn, .show-todo p{
        padding: 5px;
    }

    .show-todo input{
        margin: 7px;
    }
</style>