<script setup>
import { ref, watch } from 'vue';
// import { useRoute } from 'vue-router';
import axios from 'axios';

// const route = useRoute();

defineProps({
    myProp: {
        type: Boolean,
    },
});

const tasks = ref([
    {
        title: 'Task 1',
        description: 'this is the first task',
        completed: true,
    }
]);

async function fetchData() {
    const result = await axios.get('/api/tasks');
    tasks.value = result.data;
}

watch(() => null, fetchData, { immediate: true })

async function addTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
    };
    axios.post('/api/tasks', data)
        .then(res => alert('task was added successfully!'))
        .catch(err => alert(err.message));
    event.target.reset();
    fetchData();
}

async function deleteTask(id) {
    axios.delete(`/api/tasks/${id}`)
        .then(res => alert(`task number ${id} was deleted successfully!`))
        .catch(err => alert(err.message));
    fetchData();
}

async function updateTask(id) {
    axios.put(`/api/tasks/${id}`)
        .then(res => alert(`taask number ${id} was updated successfully!`))
        .catch(err => alert(err.message));
    fetchData();
}

</script>

<template>
    <h1>hello world</h1>
    <button type="button" @click="addTask">add task</button>
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
        <form @submit.prevent="addTask">
            <label>title:</label>
            <input type="text" name="title" required/>
            <label>description:</label>
            <input type="text" name="description"/>
            <button type="submit">Submit</button>
        </form>
    </template>
</template>
