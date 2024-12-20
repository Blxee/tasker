<script setup>
import { defineEmits, watch } from 'vue';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faPenToSquare, faTrashCan } from '@fortawesome/free-solid-svg-icons'

const emit = defineEmits();

const { task } = defineProps({
    task: {
        type: Object,
        required: true,
    }
});

// When the 'completed' field changes, commit to the database
watch(() => task.completed, () => {
    // send the data to update this task
    const data = {
        title: task.title,
        description: task.description,
        completed: task.completed == 'true',
    };
    axios.put(`/api/tasks/${task.id}`, data)
        .then(res => emit('on-toast', 'Task was updated successfully', 'success'))
        .catch(err => emit('on-toast', `Could not update task:\n${err.message}`, 'error'))
        .finally(() => emit('on-fetch-data'));
});

function deleteTask() {
    axios.delete(`/api/tasks/${task.id}`)
        .then(res => emit('on-toast', 'Task was deleted successfully', 'success'))
        .catch(err => emit('on-toast', `Could not delete task:\n${err.message}`, 'error'))
        .finally(() => emit('on-fetch-data'));
}

</script>

<template>
    <ul class="overflow-hidden shadow-xl rounded-3xl border-2 border-gray-500 aspect-square w-full bg-blue-200">
        <li class="flex bg-blue-300 p-2.5">
            <span class="font-bold me-auto truncate">{{ task.title }}</span>
            <button
                class="hover:ring-white hover:ring-2 rounded-md mx-0.5 px-0.5"
                @click="emit('on-task-update', task)"
            >
                <FontAwesomeIcon :icon="faPenToSquare" class="text-green-500" />
            </button>
            <button
                class="hover:ring-white hover:ring-2 rounded-md mx-0.5 px-0.5"
                @click="deleteTask"
            >
                <FontAwesomeIcon :icon="faTrashCan" class="text-red-500" />
            </button>
        </li>
        <li class="p-2.5">
            <div>{{ task.description }}</div>
            <div class="flex">
                <span class="me-auto justify-center">Completed:</span>
                <input
                    type="checkbox"
                    @click="(event) => { task.completed = event.target.checked }"
                    checked="task.completed"
                />
            </div>
        </li>
    </ul>
</template>
