<script setup lang="ts">

/*
    The first parameter is the event name
    and the second parameter is the payload data
*/
const emit = defineEmits<{
    (event: 'add-count', num: number ) : void;
    (event: 'reset-count') : void;
}>();
</script>

<template>
    <p class="control-bar">
        <button @click="emit('add-count', 1)">Add</button>
        <button @click="emit('reset-count')">Reset</button>
    </p>
</template>

<style scoped>
.control-bar {
    display: flex;
    justify-content: space-around;
    align-items: center;
}
</style>