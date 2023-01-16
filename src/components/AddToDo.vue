<template>
    <div class="inputs">
        <form @submit.prevent="onSubmit">
            <input type="text" v-model="title">
            <button type="submit">Create</button>
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
        display: inline-block;
    }

    input {
        width: 300px;
    }
    .inputs{
        text-align: center;
        margin: 15px 5px 15px 5px;
    }
</style>