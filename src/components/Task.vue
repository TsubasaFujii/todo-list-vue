<script setup>
    const props = defineProps({
        index: Number,
        task: Object,
    });

    const emit = defineEmits(['deleted', 'completionToggled']);

    function handleClick(index) {
        emit('deleted', index);
    }

    function handleChange() {
        emit('completionToggled');
    }
</script>

<template>
    <li
        class='task--list-item'
        :class='{completed: task.completion}'
    >
        <input
            class='task--checkbox'
            type='checkbox'
            v-model='task.completion'
            :id='task.name'
            @change='handleChange'
        />
        <label :id='task.name'> {{ task.name }} </label>
        <img
            src='./assets/rubbish-bin-icon.svg'
            @click='handleClick(index)'
            class='task--rubbish-bin icon'
        />
    </li>
</template>

<style>
    .completed {
        text-decoration: line-through;
    }
    .task--list-item {
        margin: 0.3rem 0;
        padding: var(--text-padding);

        display: flex;
        flex-direction: row;
        align-items: center;
        
        line-height: 1.2;
        border-radius: var(--border-radius);
        background-color: var(--text-background-color);
    }

    .task--list-item:hover {
        background-color: rgba(255, 255, 255, 0.9);
    }

    .task--rubbish-bin {
        width: 1.5rem;
        margin-left: auto;
        cursor: pointer;
    }

    .task--checkbox {
        filter: hue-rotate(60deg);
        transform: scale(2);
        margin: 10px;
        cursor: pointer;
    }

    .task--checkbox + label {
        overflow-wrap: break-word;
    }

    .task--checkbox:not(:checked) + label {
        cursor: pointer;
    }

    .task--checkbox:checked + label {
        text-decoration: line-through;
        color: grey;
    }
</style>