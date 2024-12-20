<script setup>
import { defineEmits } from 'vue';

const { task } = defineProps({
    task: {
        type: Object,
        required: true,
    }
});

const emit = defineEmits();

async function updateTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
    };

    // send the data to update this task
    axios.put(`/api/tasks/${task.id}`, data)
        .then(res => alert('task was updated successfully!'))
        .catch(err => alert(err.message))
        .finally(() => emit('form-submitted'));
}

</script>

<template>
    <form id="taskForm" @submit.prevent="updateTask">
        <legend>Edit Task</legend>

        <label>title:</label>
        <input type="text" name="title" :value="task.title" placeholder="Title" required/>

        <label>description:</label>
        <input type="text" name="description" :value="task.description" placeholder="Description"/>

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
