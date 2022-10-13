<script setup>
import { ref, watch } from 'vue';

    const inputValue = ref('');
    const hasValidInput = ref(false);
    const props = defineProps({
        hasCompletedTask: Boolean,
    })

    const emit = defineEmits(['addTask', 'removeCompletedTasks']);

    watch(inputValue, () => {
        if(inputValue.value.trim().length === 0 || inputValue.value === '') {
            hasValidInput.value = false;
        } else {
            hasValidInput.value = true;
        }
    });

    function handleAdd() {
        // Input only contains white space isn't accepted
        if(inputValue.value.trim().length === 0) return;
        const newTask = {
            name: inputValue.value,
            completion: false,
        };
        emit('addTask', newTask);

        // reset input field
        inputValue.value = '';
    }

    function handleRemoveAllCompleted() {
        emit('removeCompletedTasks');
    }
</script>

<template>
    <div class='input-handler--wrapper'>
        <div class='input-handler--input'>
            <input
                class='input-handler--input-field'
                type='text'
                placeholder='Enter a task'
                v-model='inputValue'
                @keydown.enter='handleAdd'
            />
            <button
                class='input-handler--button input-handler--add-button'
                @click='handleAdd'
                :disabled='!hasValidInput'
            >
                    <img
                        src='./assets/addButton.svg'
                        class='input-handler--add-button-img'
                    >
            </button>
        </div>
        <button
            class='input-handler--button input-handler--clear-button'
            @click='handleRemoveAllCompleted'
            :disabled='!hasCompletedTask'
        >
            Remove completed tasks
        </button>
    </div>
</template>

<style>
    .input-handler--wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 1rem;
    }

    /* input field */
    .input-handler--input {
        display: flex;
        flex-direction: row;
        align-items: center;
    }
    .input-handler--input-field {
        /* typography: under index.css */
        margin: auto;
        padding: var(--text-padding);

        border-radius: var(--border-radius);
        border: none;
        background-color: rgba(255, 255, 255, 0.5);
        border: 3px rgba(0, 0, 0, 0) solid;
        transition: all 0.5s ease-out;
    }
    .input-handler--input-field:focus {
        outline: none;
        background-color: rgba(255, 255, 255, 0.6);
        border: 3px black solid;
    }

    /* buttons */
    .input-handler--button {
        border: none;
        background-color: var(--button-color);
        cursor: pointer;
        box-shadow: 0.2rem 0.2rem 0.2rem rgba(0, 0, 0, 0.425);
    }
    .input-handler--add-button {
        width: 2.5rem;
        height: 2.5rem;
        margin: 0.5rem;

        border-radius: 50%;
        display: inline-block;
    }
    .input-handler--add-button-img {
        width: 1.5rem;
        vertical-align: middle;
    }
    .input-handler--clear-button {
        /* typography: under index.css */
        width: 100%;
        margin: auto;
        padding: var(--text-padding);
        border-radius: var(--border-radius);
    }
    .input-handler--clear-button:disabled,
    .input-handler--add-button:disabled {
        cursor: not-allowed;
        box-shadow: none;
        filter: grayscale(60%);
    }
    .input-handler--clear-button:not(:disabled):hover,
    .input-handler--add-button:not(:disabled):hover {
        transform: translate(0.1rem);
        box-shadow: none;
    }
</style>