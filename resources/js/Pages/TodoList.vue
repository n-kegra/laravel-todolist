<script setup>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated.vue";
import BreezeLabel from "@/Components/Label.vue";
import BreezeInput from "@/Components/Input.vue";
import BreezeInputError from "@/Components/InputError.vue";
import BreezeButton from "@/Components/Button.vue";
import { Head, useForm } from "@inertiajs/inertia-vue3";
import { reactive } from "vue";

defineProps({
    todos: {
        uuid: String,
        title: String,
        deadline: String,
    },
});

const form = useForm({
    title: "",
    deadline: "",
});

const add_todo = () => {
    form.post(route("todos.store"));
};

const done_todo = ($uuid) => {
    useForm({}).delete(route('todos.destroy', $uuid));
}

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
                    <form
                        class="p-6 bg-white border-b border-gray-200"
                        @submit.prevent="add_todo"
                    >
                        <div>
                            <BreezeLabel for="todo" value="Todo:" />
                            <BreezeInput
                                id="todo"
                                type="text"
                                class="mt-1 block w-80"
                                v-model="form.title"
                                required
                            />
                            <BreezeInputError
                                class="mt-1 block w-80"
                                :message="form.errors.title"
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
                        <BreezeButton
                            class="mt-2"
                            :class="{ 'opacity-25': form.processing }"
                            :disabled="form.processing"
                            >Add</BreezeButton
                        >
                    </form>
                    <div class="p-6 bg-white border-b border-gray-200">
                        <table>
                            <tbody>
                                <tr>
                                    <th>title</th>
                                    <th>deadline</th>
                                    <th>action</th>
                                </tr>
                                <tr v-for="todo in todos" :key="todo.uuid">
                                    <td class="p-2">{{ todo.title }}</td>
                                    <td class="p-2">{{ todo.deadline }}</td>
                                    <td class="p-2">
                                        <form @submit.prevent="done_todo(todo.uuid)">
                                            <BreezeButton>Done</BreezeButton>
                                        </form>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </BreezeAuthenticatedLayout>
</template>
