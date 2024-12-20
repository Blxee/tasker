<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import AddTaskForm from '@/Components/AddTaskForm.vue';
import UpdateTaskForm from '@/Components/UpdateTaskForm.vue';
import Toast from '@/Components/Toast.vue';
import TaskCard from '@/Components/TaskCard.vue';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faFeather, faBook } from '@fortawesome/free-solid-svg-icons'

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
    <div class="text-xl">
        <div class="text-7xl bg-gray-200 py-4 font-extrabold text-center text-gray-800">
            <FontAwesomeIcon :icon="faBook" class="aspect-square" size="xl" />
            Tasker
        </div>
        <div v-if="tasks && tasks.length > 0" class="m-2">
            <h1>Tasks:</h1>
            <div class="grid grid-cols-2 md:grid-cols-5 gap-2">
                <TaskCard
                    v-for="task in tasks"
                    v-bind:key="task.id"
                    :task="task"
                    @on-task-update="openUpdateForm"
                    @on-fetch-data="fetchData"
                    @on-toast="showToast"
                />
            </div>
        </div>
        <div v-else>
            You have no task yet!<br>
            Press <b>Add Task</b> to create one!
        </div>

        <div class="w-full bg-gray-200 px-4 py-0.5 flex items-center">
            <button
                :disabled="pagination.currentPage <= 1"
                @click="pagination.currentPage--"
                class="ms-auto rounded-lg border-2 border-blue-900 disabled:opacity-50 bg-blue-400 hover:bg-blue-600 px-3 py-1 m-3"
            >Prev</button>

            <span class="font-lg font-bold">{{ pagination.currentPage }}</span>

            <button
                :disabled="pagination.currentPage >= pagination.lastPage"
                @click="pagination.currentPage++"
                class="me-auto rounded-lg border-2 border-blue-900 disabled:opacity-50 bg-blue-400 hover:bg-blue-600 px-3 py-1 m-3"
            >Next</button>
        </div>

        <UpdateTaskForm v-if="updateForm" :task="taskEdited" @on-toast="showToast" @form-submitted="onFormSubmit"/>

        <AddTaskForm v-if="addForm" @on-toast="showToast" @form-submitted="onFormSubmit"/>

        <Toast ref="toastRef" />

        <button
            @click="addForm = true"
            class="p-5 aspect-square rounded-full fixed bottom-3 right-3 bg-blue-500 text-blue-900 text-2xl"
        >
            <FontAwesomeIcon :icon="faFeather" class="aspect-square" size="xl" />
        </button>
    </div>
</template>
