<template>
    <div class="flex space-x-4">
        <button @click="emit('close')" type="button" class="inline-flex w-full justify-center font-mono rounded-md bg-red-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-red-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-red-600">Cancel</button>
        <button @click="emit('submit')" type="button" class="inline-flex w-full justify-center font-mono rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">{{ name }}</button>
    </div>
</template>

<script setup lang="ts">
const emit = defineEmits(['close', 'submit'])

const props = defineProps({
    name: String
})
</script>