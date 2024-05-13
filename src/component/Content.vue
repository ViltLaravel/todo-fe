<template>
    <div class="bg-[#EEEEEE] sm:p-[40px] rounded-t-md shadow-lg">
        <ol class="font-mono font-[400]">
            <ul v-for="todos in todo.data" :key="todos.id" class="flex justify-between sm:p-2">
                <li>{{ todos.name }}</li>
                <div class="space-x-2 sm:pt-1">
                    <button @click="editTodo(todos.id)" class="w-4 text-gray-500 hover:text-gray-600"><PencilAltIcon /></button>
                    <button @click="deleteTodo(todos.id)" class="w-4 text-red-500 hover:text-red-600"><TrashIcon /></button>
                </div>
            </ul>
        </ol>
    </div>
    <div class="flex justify-center">
        <div class="sm:w-[120px] bg-[#7469B6] hover:bg-violet-500 absolute -m-5 sm:px-3 sm:py-2 rounded-[50px] shadow-lg">
            <button @click="clickTodo" class="items-center font-[600] font-mono text-gray-300">+ New Task</button>
        </div>
    </div>
    <div>
        <Pagination :to="pagination.value.to" :total="pagination.value.total" :from="pagination.value.from" :links="pagination.value.links" @paginate="fetchTodo" />
    </div>
    <Modal :show="show" @close="unclickTodo" :title="title" >
        <FormInput v-model="modeldata.name" />
        <SubmitButton @close="unclickTodo" @submit="submit" :name="nameButton" />
    </Modal>
</template>

<script setup lang="ts">
import {TrashIcon, PencilAltIcon } from 'heroicons-vue3/outline'
import axios from 'axios';
import { onMounted, reactive, ref, watch, type PropType } from 'vue';

// @ts-ignore
import type {TTodo} from '../../types/todo.ts';
// @ts-ignore
import SubmitButton from '@/component/Button.vue'
// @ts-ignore
import FormInput from '@/component/FormInput.vue'
// @ts-ignore
import Modal from '@/component/Modal.vue'
// @ts-ignore
import Pagination from '@/component/Pagination.vue'

let modeldata = reactive({
    id: '',
    name: ''
})

const pagination: any = reactive({
    value: {
        links: [],
        to: 0,
        total: 0,
        from: 0,
    }
})

const show = ref(false)
const clickTodo = () => {
    show.value = true
    submitting.value = true;
    editing.value = false;
}
const unclickTodo = () => {
    show.value = false
}

const todo: TTodo = reactive({
    data: []
});

let title = ref('');
let nameButton = ref('')
let submit = ref()
const submitting = ref(false);
const editing = ref(false);

const titleTodo = () => {
    if (submitting.value) {
        title.value = 'Create New Task';
        nameButton.value = 'Create Task';
        submit.value = submitTodo;
    } else if (editing.value) {
        title.value = 'Edit Task';
        nameButton.value = 'Update Task';
        submit.value = () => updateTodo(modeldata.id, modeldata);
    } else {
        title.value = 'Task List';
        nameButton.value = 'Save';
    }
}

const fetchTodo = async (url: string | null) => {
    const link = ref('https://todo-production-2d63.up.railway.app/api/todo' || 'http://todo-production-2d63.up.railway.app/api/todo')
    if (url !== null) {
        link.value = url
    }
    try {
        const res = await axios.get(link.value);
        if (res) {
            todo.data = res.data.data
            pagination.value = res.data
            console.log(pagination, 'papapap')
        }
    } catch (error) {
        console.log('Failed fetching todo :', error)
    }
}

const submitTodo = async () => {
    try {
        const res = await axios.post('https://todo-production-2d63.up.railway.app/api/todo', modeldata);
        if (res.status === 200) {
            show.value = false;
            modeldata.name = '';
            fetchTodo(null);
        }
    } catch (error) {
        console.log('Error adding todo:', error);
    }
}

const editTodo = async (id: string) => {
    try {
        const res = await axios.get(`https://todo-production-2d63.up.railway.app/api/todo/${id}`);
        if (res.status === 200) {
            show.value = true;
            modeldata = res.data;
            fetchTodo(null);
            submitting.value = false;
            editing.value = true;
        }
    } catch (error) {
        console.log('Error encountered:', error);
    }
}

const updateTodo = async (id: string, modeldata: any) => {
    try {
        const res = await axios.put(`https://todo-production-2d63.up.railway.app/api/todo/${id}/update`, modeldata);
        if (res) {
            show.value = false;
            modeldata.name = '';
            fetchTodo(null);
        }
    } catch (error) {
        console.log('Error updating todo :', error)
    }
}

const deleteTodo = async (id: string) => {
    try {
        const res = await axios.delete(`https://todo-production-2d63.up.railway.app/api/todo/${id}`);
        if(res) {
            fetchTodo(null);
        }
    } catch (error) {
        console.log('Error deleting todo :', error)
    }
}

onMounted(async () => {
    fetchTodo(null);
})

watch([submitting, editing], () => {
    titleTodo();
});

</script>

<style scoped>

</style>