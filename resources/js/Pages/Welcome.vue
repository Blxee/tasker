<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import AddTaskForm from '@/Components/AddTaskForm.vue';

const tasks = ref([
    {
        title: 'Task 1',
        description: 'this is the first task',
        completed: true,
    }
]);

const addForm = ref(false);
const updateForm = ref(false);

function closeForms() {
    addForm.value = false;
    updateForm.value = false;
}

async function fetchData() {
    const result = await axios.get('/api/tasks');
    tasks.value = result.data;
}

watch(() => null, fetchData, { immediate: true })

async function deleteTask(id) {
    axios.delete(`/api/tasks/${id}`)
        .then(res => alert(`task number ${id} was deleted successfully!`))
        .catch(err => alert(err.message));
    location.reload()
}

async function updateTask(id) {
    axios.put(`/api/tasks/${id}`)
        .then(res => alert(`taask number ${id} was updated successfully!`))
        .catch(err => alert(err.message));
    location.reload()
}

</script>

<template>
    <button type="button" @click="addTask">add task</button>
    <template v-if="tasks">
        <h1>Tasks:</h1>
        <ol :key="refreshKey">
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
