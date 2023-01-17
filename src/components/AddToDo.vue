<template>
    <div class="add-form">
        <form @submit.prevent="onSubmit">
            <input type="text" v-model="title" placeholder="Add todos here...">
            <button type="submit" class="btn">Create</button>
        </form>
    </div>    
</template>

<script>
export default {
    data() {
        return {
            title: ''
        }
    },
    methods: {
        onSubmit() {
            if (this.title.trim()) {
                const newToDo = {
                    id: Date.now(),
                    title: this.title,
                    completed: false
                }
                this.$emit('add-todo', newToDo)
                this.$emit('exist-todo', newToDo)
                this.title = ''
            }
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
    }

    input {
        width: 100%;
        border: 1px solid #008080;
        padding: 10px 15px;
        border-radius: 4px;
    }
    .add-form{
        text-align: center;
        padding: 15px;
    }

    .btn{
        padding: 10px 15px;
        background: none;
        color: darkslategray;
        border: 1px solid black;
        border-radius: 4px;
        margin-left: 5px;
    }
</style>