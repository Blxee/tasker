<script setup>
import { defineEmits } from 'vue';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faXmark } from '@fortawesome/free-solid-svg-icons'

const { task } = defineProps({
    task: {
        type: Object,
        required: true,
    }
});

const emit = defineEmits();

function updateTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
        completed: Boolean(formData.get('completed')),
    };

    // send the data to update this task
    axios.put(`/api/tasks/${task.id}`, data)
        .then(res => emit('on-toast', 'Task was updated successfully', 'success'))
        .catch(err => emit('on-toast', `Could not update task:\n${err.message}`, 'error'))
        .finally(() => emit('form-submitted'));
}

</script>

<template>
    <form id="taskForm" @submit.prevent="updateTask" class="rounded-xl shadow-xl border-2 border-gray-800 p-6 flex flex-col">
        <div class="flex items-center">
            <legend class="font-lg font-bold mx-auto">Update Task</legend>
            <FontAwesomeIcon :icon="faXmark" @click="emit('form-submitted')" class="hover:ring-white hover:ring-2 rounded-md mx-0.5 px-0.5" />
        </div>

        <hr class="border-lg border-gray-800 mb-4 mt-2"/>

        <div class="flex">
            <label>Title:</label>
            <input class="ms-auto rounded-lg" type="text" name="title" :value="task.title" placeholder="Title" required/>
        </div>

        <div class="flex mt-3">
            <label>Description:</label>
            <input class="ms-auto rounded-lg" type="text" name="description" :value="task.description" placeholder="Description" />
        </div>

        <div class="flex mt-3">
            <label>Completed:</label>
            <input class="ms-auto rounded-lg" type="checkbox" name="completed" :checked="task.completed" />
        </div>

        <hr class="border-lg border-gray-800 mb-3 mt-3"/>

        <button type="submit" class="mx-auto rounded-lg border-2 border-gray-900 disabled:opacity-50 bg-gray-400 hover:bg-gray-600 px-5 py-1 m-1">Submit</button>
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
