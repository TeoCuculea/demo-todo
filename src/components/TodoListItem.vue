<script setup>
defineProps({
    userId: Number,
    id: Number,
    title: String,
    completed: Boolean
})
</script>

<template>
    <p class="grid-item">{{userId}}</p>
    <input class="grid-item" type="checkbox" :checked="completed" />
    <p class="grid-item">{{ title }}</p>
    <button >Delete</button>
</template>

<style scoped>
.grid-item{
    display:inline-block;
    text-align: center;
}
</style>