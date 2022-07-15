<script setup>
import { computed, ref } from 'vue';
import printTodoItem from '../utils/printTodoItem';

const props = defineProps({//returneaza obiect cu proprietati reactive
    id: Number,
    userId: Number,
    title: String,
    completed: Boolean
});//toate proprietatile sunt reactive, trimit in jos datele prin proprietati ( spre copii ), trimiti in sus cu evenimente ( spre parinti )


const emit = defineEmits(['todoItemDeleted','todoItemCompleted']);//definim evenimnte ce pot fi ascuktate de afara

//const newTitle = computed(()=> `${props.id}. ${props.tile}[${props.completed}]`);
const newTitle = computed(() => printTodoItem(props.id, props.title, props.completed));
function handleChange(event){
    emit('todoItemCompleted', event.target.checked);//ev cu payload
}

</script>

<template>
    <p class="grid-item">{{userId}}</p>
    <input class="grid-item" type="checkbox" :checked="completed" @change="handleChange"/>
    <p class="grid-item">{{ newTitle }}</p>
    <button v-on:click="$emit('todoItemDeleted')" class="grid-item">Delete</button> <!-- @ in loc de v-on face acelasi lucru, ev fara payload -->
</template>

<style scoped>
.grid-item{
    display:inline-block;
    text-align: center;
}
</style>