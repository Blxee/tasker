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
    tasks.value = await axios.get('/api/tasks');
}

watch(() => null, fetchData, { immediate: true })

async function addTask(event) {
    const formData = new FormData(event.target);
    const data = {
        title: formData.get('title'),
        description: formData.get('description'),
    };
    const result = await axios.post('/api/tasks', data);
    event.target.reset();
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
