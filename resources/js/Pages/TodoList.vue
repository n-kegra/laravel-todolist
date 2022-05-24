<script setup>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated.vue";
import BreezeLabel from "@/Components/Label.vue";
import BreezeInput from "@/Components/Input.vue";
import BreezeInputError from "@/Components/InputError.vue";
import BreezeButton from "@/Components/Button.vue";
import { Head, useForm } from "@inertiajs/inertia-vue3";
import { reactive } from "vue";

const form = useForm({
    todo_title: "",
    deadline: "",
});

const todos = reactive([]);

const add_todo = () => {
    form.post(route("api-addtodo"), {
        onSuccess: () => {
            // todos.push({
            //     id: ,
            //     title: ,
            // });
        },
    });
};
</script>

<template>
    <Head title="TodoList" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                TodoList
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 bg-white border-b border-gray-200">
                        <div>
                            <BreezeLabel for="todo" value="Todo:" />
                            <BreezeInput
                                id="todo"
                                type="text"
                                class="mt-1 block w-80"
                                v-model="form.todo_title"
                                required
                            />
                            <BreezeInputError
                                class="mt-1 block w-80"
                                :message="form.errors.todo_title"
                            />
                        </div>
                        <div>
                            <BreezeLabel for="deadline" value="Deadline:" />
                            <BreezeInput
                                id="deadline"
                                type="text"
                                class="mt-1 block w-80"
                                v-model="form.deadline"
                                required
                            />
                            <BreezeInputError
                                class="mt-1 block w-80"
                                :message="form.errors.deadline"
                            />
                        </div>
                        <BreezeButton class="mt-1" @click="add_todo"
                            >Add</BreezeButton
                        >
                    </div>
                    <div class="p-6 bg-white border-b border-gray-200">
                        <ul>
                            <li v-for="todo in todos" :key="todo.id">
                                {{ todo.title }}
                                <BreezeButton>Done</BreezeButton>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </BreezeAuthenticatedLayout>
</template>
