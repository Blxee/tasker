<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import AddTaskForm from '@/Components/AddTaskForm.vue';

// The tasks list
const tasks = ref([]);

// These determine whether a form is still open
const addForm = ref(false);
const updateForm = ref(false);

async function fetchData() {
    const result = await axios.get('/api/tasks');
    tasks.value = result.data;
}

// Close the form after submitting
function closeForms() {
    addForm.value = false;
    updateForm.value = false;
    fetchData();
}

watch(() => null, fetchData, { immediate: true })

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
                    <button @click="updateTask(task.id)">update</button>
                </ul>
            </li>
        </ol>
    </template>

    <AddTaskForm v-if="addForm" @form-submitted="closeForms"/>
    <button @click="addForm = true">Add Task</button>

</template>
