<script setup>
import { defineEmits } from 'vue';
import axios from 'axios';

const emit = defineEmits();

function addTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
    };
    // send the data to create a new task
    axios.post('/api/tasks', data)
        .then(res => emit('on-toast', 'Task was created successfully', 'success'))
        .catch(err => emit('on-toast', `Could not create task:\n${err.message}`, 'error'))
        .finally(() => emit('form-submitted'));
    // Reset the form fields
    event.target.reset();
}

</script>

<template>
    <form id="taskForm" @submit.prevent="addTask">
        <legend>Create Task</legend>

        <label>title:</label>
        <input type="text" name="title" placeholder="Title" required/>

        <label>description:</label>
        <input type="text" name="description" placeholder="Description"/>

        <button type="submit">Submit</button>
    </form>
</template>

<style>

#taskForm {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: gray;
    padding: 1rem;
}

</style>
