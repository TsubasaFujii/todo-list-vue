<script setup>
import Task from './Task.vue';

    const props = defineProps({
        tasks: Array,
    })
    const emit = defineEmits(['removeTask', 'sortTaskList']);

    function handleDelete(index) {
        emit('removeTask', index);
    }

    function handleToggleCompletion() {
        emit('sortTaskList');
    }
</script>

<template>
    <ul class='to-do-list'>
        <li
            v-if='tasks.length === 0'
            class='no-task-label no-task-msg'
        >
            No tasks
        </li> 
        <Task
            v-else
            v-for='(task, index) in tasks'
            :key='index'
            :task='task'
            :index='index'
            @deleted='handleDelete'
            @completionToggled='handleToggleCompletion'
        />
    </ul>
</template>

<style>
    .to-do-list {
        width: 100%;
        list-style-type: none;
        margin: 0;
        padding: 0;
    }
    .no-task-label {
        /* typography(.no-task-msg): under index.css */
        padding: 2rem;

        border-radius: 0.2rem;
        background-color: rgba(255, 255, 255, 0.4);
        text-align: center;
    }
</style>