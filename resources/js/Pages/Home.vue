<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import AddTaskForm from '@/Components/AddTaskForm.vue';
import UpdateTaskForm from '@/Components/UpdateTaskForm.vue';
import Toast from '@/Components/Toast.vue';
import TaskCard from '@/Components/TaskCard.vue';

// The tasks list
const tasks = ref([]);
// pagination value
const pagination = ref({
    currentPage: 1,
    lastPage: -1,
});
// These determine whether a form is still open
const addForm = ref(false);
const updateForm = ref(false);
// Reference to the task that is currently being edited
const taskEdited = ref(null);

const toastRef = ref(null);

// Fetch data when component mounts and whenever the page changes
watch(() => pagination.value.currentPage, fetchData, { immediate: true })

// Retrieve the data from the backend
async function fetchData() {
    try {
        const result = await axios.get(`/api/tasks?page=${pagination.value.currentPage}`);
        tasks.value = result.data.data;
        // pagination.value.currentPage = result.data.current_page;
        pagination.value.lastPage = result.data.last_page;
    } catch (error) {
        showToast(`Could not retrieve tasks:\n${error.message}`);
    }
}

// Close the form after submitting
function onFormSubmit() {
    addForm.value = false;
    updateForm.value = false;
    taskEdited.value = null;
    fetchData();
}

function openUpdateForm(task) {
    // Show the update task form
    updateForm.value = true;
    // Pre-fill the form with the original task data
    taskEdited.value = task;
}

function showToast(message, type) {
    toastRef.value?.show(message, type);
}

</script>

<template>
    <div class="text-7xl">Tasker</div>
    <template v-if="tasks && tasks.length > 0">
        <h1>Tasks:</h1>
        <ol>
            <li v-for="task in tasks" v-bind:key="task.id">
                <TaskCard :task="task" @on-task-update="openUpdateForm" @on-task-delete="fetchData" @on-toast="showToast" />
            </li>
        </ol>
    </template>
    <template v-else>
        You have no task yet!<br>
        Press <b>Add Task</b> to create one!
    </template>

    <div>
        <button
            :disabled="pagination.currentPage <= 1"
            @click="pagination.currentPage--"
        >prev</button>

        <span>{{ pagination.currentPage }}</span>

        <button
            :disabled="pagination.currentPage >= pagination.lastPage"
            @click="pagination.currentPage++"
        >next</button>
    </div>

    <UpdateTaskForm v-if="updateForm" :task="taskEdited" @on-toast="showToast" @form-submitted="onFormSubmit"/>

    <AddTaskForm v-if="addForm" @on-toast="showToast" @form-submitted="onFormSubmit"/>
    <button @click="addForm = true">Add Task</button>

    <Toast ref="toastRef" />
</template>
