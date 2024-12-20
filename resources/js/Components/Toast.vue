<script setup>

import { ref } from 'vue';

const visible = ref(false);
const toastMessage = ref('');
const toastType = ref('info');

// This will be called from the parent using ref to show the Toast
function show(message, type) {
    visible.value = true;
    toastMessage.value = message;
    toastType.value = type || 'info';
    setTimeout(() => visible.value = false, 3000);
    // animate
}

defineExpose({ show });
</script>

<template>
    <div v-if="visible" class="absolute bottom-0"
        :class="{
            'bg-green-500': toastType === 'success',
            'bg-red-500': toastType === 'error',
            'bg-blue-500': toastType === 'info'
        }"
    >{{ toastMessage }}</div>
</template>
