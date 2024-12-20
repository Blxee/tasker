<script setup>
import { defineEmits } from 'vue';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faHouse } from '@fortawesome/free-solid-svg-icons'

const emit = defineEmits();

const { task } = defineProps({
    task: {
        type: Object,
        required: true,
    }
});

async function deleteTask() {
    axios.delete(`/api/tasks/${task.id}`)
        .then(res => emit('on-toast', 'Task was deleted successfully', 'success'))
        .catch(err => emit('on-toast', `Could not delete task:\n${err.message}`, 'error'))
        .finally(() => emit('on-task-delete'));
}

</script>

<template>
    <ul class="rounded-xl border-2 border-gray-500 aspect-square w-32 m-2 p-2.5">
        <FontAwesomeIcon :icon="faHouse" />
        <li>Title: {{ task.title }}</li>
        <li>Description: {{ task.description }}</li>
        <li>Completed: {{ task.completed }}</li>
        <button @click="deleteTask">delete</button>
        <button @click="emit('on-task-update', task)">update</button>
    </ul>
</template>
