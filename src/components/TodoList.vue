<script setup>
import TodoListItem from './TodoListItem.vue';
import { ref, reactive, toRefs, watch, computed } from 'vue'

const response = await fetch('https://jsonplaceholder.typicode.com/todos');
let todos = await response.json();
todos = todos.slice(0, 10);
const reactiveTodos = ref(todos);//vue va reranda automat componenta, ca sa avem acces .value

const counter = reactive({
    deleted: 0,
    updated: 0,
    ct: todos[todos.length - 1].id
})


/*const counter2 = {// nu pot destructura => nu mai e reactiv, ca sa pastrez proxy ca sa il trimit mai departe il destructurez in refuri
    ...toRefs(counter),
    total: 0
}
function getUpdatedCounter() {// la fel face ca functia () de mai jos
    return counter.updated;
}*/

//const counter = ref(0);

watch(() => counter.updated, (newValue, oldValue) => { //prima parte ce urmarim functia care tot verifica daca s-a modifica
    //, a doua ce se executa
    console.log(`Counter updated ${oldValue} -> ${newValue}`);
});

function handleTodoItemDeleted(todoItemId) {
    console.log(`item deleted: ${todoItemId}`);
    reactiveTodos.value = reactiveTodos.value.filter(item => item.id !== todoItemId);
    counter.deleted++;
    //counter.value++;
}

function handleTodoItemCompleted(todoItemId, completed) {
    reactiveTodos.value = reactiveTodos.value.map(todo => {
        if (todo.id === todoItemId) {
            return {
                ...todo,
                completed: completed
            }
        } else {
            return todo;
        }
    });
    console.log(`item updated: ${todoItemId} >> ${completed}`);
    counter.updated++;
    //counter.value++;
}

//monitorizare obiect - salvam, logam ceva
const condition = computed(() => userId.value != 0 && userId.value != null && title.value != "");
const checkbox = ref(true);//primeste valoarea de afara
const userId = ref(0);
const title = ref("");
const check = ref(true);
watch(checkbox, newValue => {
    console.log(`checkbox: ${newValue}`);
});

function handleTodoItemAdded() {
    const newTodo = {
        id: ++counter.ct,
        userId: userId.value,
        title: title.value,
        completed: check.value
    }
    reactiveTodos.value.push(newTodo);
    title.value = "";
    userId.value = 0;
    //reactiveTodos.value.splice(0,0,newTodo);
}


const emit = defineEmits(['todoItemSubmit']);

</script>

<template>
    <span>Two-way data binding (v-model):</span>
    <p>Changes [deleted: {{ counter.deleted }}, changed: {{ counter.updated }}]</p>
    <div class="form">
        <form @submit.prevent='handleTodoItemAdded'>
            <label class="label" for="userid">User ID:</label><br>
            <input class="input" type="number" id="userid" name="userid" v-model="userId" required><br>
            <label class="label" for="completed">Completed:</label><br>
            <input class="input" type="checkbox" name="completed" v-model="check" id="completed"><br>
            <label class="label" for="title">Title:</label><br>
            <input class="input" type="text" id="title" name="title" v-model="title" required><br>
            <button :disabled="!condition" type="submit" value="Submit">Submit</button>
        </form>
    </div>
    <div class="grid-container">
        <TodoListItem v-for="todo of reactiveTodos" :key="todo.id" :id="todo.id" :title="todo.title"
            :completed="todo.completed" :userId="todo.userId"
            @todo-item-completed="completed => handleTodoItemCompleted(todo.id, completed)"
            @todo-item-deleted="handleTodoItemDeleted(todo.id)" />
    </div>
</template>

<style scoped>
.grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-gap: 20px;
    margin: auto;
    width: 90%;
    border: 3px solid green;
    padding: 20px;
}

.form {
    margin: auto;
    width: 50%;
    padding: 10px;
}
</style>