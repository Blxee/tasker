<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import AddTaskForm from '@/Components/AddTaskForm.vue';
import UpdateTaskForm from '@/Components/UpdateTaskForm.vue';
import Toast from '@/Components/Toast.vue';

// The tasks list
const tasks = ref([]);

// These determine whether a form is still open
const addForm = ref(false);
const updateForm = ref(false);
// Reference to the task that is currently being edited
const taskEdited = ref(null);

const toastRef = ref(null);

async function fetchData() {
    const result = await axios.get('/api/tasks');
    tasks.value = result.data;
}

// Close the form after submitting
function onFormSubmit() {
    addForm.value = false;
    updateForm.value = false;
    taskEdited.value = null;
    fetchData();
}

onMounted(() => {
    fetchData();
});

async function deleteTask(id) {
    axios.delete(`/api/tasks/${id}`)
        .then(res => alert(`task number ${id} was deleted successfully!`))
        .catch(err => alert(err.message))
        .finally(fetchData);
}

async function updateTask(id) {
    axios.put(`/api/tasks/${id}`)
        .then(res => alert(`taask number ${id} was updated successfully!`))
        .catch(err => alert(err.message))
        .finally(fetchData);
}

function showToast(message, type) {
    toastRef.value?.show(message, type);
}
</script>

<template>
    <template v-if="tasks">
        <h1>Tasks:</h1>
        <ol>
            <li v-for="task in tasks" v-bind:key="task.id">
                <ul>
                    <li>Title: {{ task.title }}</li>
                    <li>Description: {{ task.description }}</li>
                    <li>Completed: {{ task.completed }}</li>
                    <button @click="deleteTask(task.id)">delete</button>
                    <button @click="updateForm = true; taskEdited = task">update</button>
                </ul>
            </li>
        </ol>
    </template>

    <AddTaskForm v-if="addForm" @form-submitted="onFormSubmit"/>
    <button @click="addForm = true">Add Task</button>

    <UpdateTaskForm v-if="updateForm" :task="taskEdited" @form-submitted="onFormSubmit"/>

    <Toast ref="toastRef" />
    <button @click="showToast('hello toast', 'success')">Toaaaaast-----------------</button>
</template>
