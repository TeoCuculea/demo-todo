<script setup>
import TodoListItem from './TodoListItem.vue';

const response = await fetch('https://jsonplaceholder.typicode.com/todos');
const todos = await response.json();
const nameVar = "def";
</script>

<template>
    <div class="form">
        <form action>
            <label class="label" for="userid">User ID:</label>
            <input class="input" type="number" id="userid" name="userid"><br>
            <label class="label" for="completed">Completed:</label>
            <input class="input" type="checkbox" id="completed" name="completed"><br>
            <label class="label" for="title">Title:</label>
            <input class="input" type="text" id="title" name="title"><br>

        </form>
    </div>
    <div class="grid-container">
        <TodoListItem
            v-for="todo of todos"
            :key="todo.id"
            :id="todo.id"
            :title="todo.title"
            :completed="todo.completed"
            :userId="todo.userId"
        />
    </div>
</template>

<style scoped>
    .grid-container{
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        grid-gap:20px;
        margin: auto;
        width: 90%;
        border: 3px solid green;
        padding: 20px;
    }
    .form{
            margin: auto;
            width: 50%;
            padding: 20px;
    }
    .input{
        padding-left:30px;
    }
</style>