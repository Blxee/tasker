<script setup>
import { defineEmits } from 'vue';
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

async function deleteTask() {
    axios.delete(`/api/tasks/${task.id}`)
        .then(res => emit('on-toast', 'Task was deleted successfully', 'success'))
        .catch(err => emit('on-toast', `Could not delete task:\n${err.message}`, 'error'))
        .finally(() => emit('on-task-delete'));
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
            <div>{{ task.completed ? "Completed" : "Uncompleted" }}</div>
        </li>
    </ul>
</template>
