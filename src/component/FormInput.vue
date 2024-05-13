<template>
    <input v-model="model" type="text" name="name" id="name" class="block px-4 w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-1 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" placeholder="e.g. I want to go to church." />
</template>

<script setup lang="ts">
const model = defineModel<any>({ required: true })
</script>