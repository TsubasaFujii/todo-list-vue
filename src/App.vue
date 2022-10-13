<script setup>
import { ref } from 'vue';

import InputHandler from './components/InputHandler.vue';
import TaskList from './components/TaskList.vue';

    const tasks = ref([]);

    function addTask(newTask) {
        tasks.value.push(newTask);
    }

    function removeTask(target) {
        tasks.value.splice(target, 1);
    }

    function sortTasks(target) {
        tasks.value.sort(sortByCompletion);

        function sortByCompletion(a, b) {
            if(!a.completion && b.completion) {
                return -1;
            } else if(a.completion && !b.completion) {
                return 1;
            } else {
                return 0;
            }
        }
    }

    function removeCompletedTasks() {
        const targets = tasks.value.reduce((results, current, currentIndex) => {
            if(current.completion){
                // Creating a reversed array to delete tasks backwards later
                results.unshift(currentIndex);
            }
            return results;
        }, []);
        targets.forEach((target) => {
            tasks.value.splice(target, 1);
        });
    }
</script>

<template>
    <h1>My to-do list</h1>
    <main>
        <InputHandler
            @addTask='addTask'
            @removeCompletedTasks='removeCompletedTasks'
            :hasCompletedTask='tasks.some((task) => task.completion)'
        />
        <TaskList :tasks='tasks' @removeTask='removeTask' @sortTaskList='sortTasks' />
    </main>
</template>

<style>
    main {
        max-width: 30rem;
        margin: 0 auto;

        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
    }

    h1 {
        text-align: center;
    }
</style>

<!--
class binding
https://vuejs.org/guide/essentials/class-and-style.html#binding-html-classes
event handling
https://vuejs.org/guide/essentials/event-handling.html
child component
https://vuejs.org/guide/essentials/component-basics.html#passing-props
-->