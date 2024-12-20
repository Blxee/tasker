<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits();

async function addTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
    };
    // send the data to create a new task
    axios.post('/api/tasks', data)
        .then(res => alert('task was added successfully!'))
        .catch(err => alert(err.message))
        .finally(() => emit('form-submitted'));
    // location.reload()
    // Reset the form fields
    event.target.reset();
}

</script>

<template>
    <form id="taskForm" @submit.prevent="addTask">
        <legend>Create Task</legend>
        <label>title:</label>
        <input type="text" name="title" required/>
        <label>description:</label>
        <input type="text" name="description"/>
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
