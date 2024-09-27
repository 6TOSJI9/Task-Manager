<template>
    <div>
        <h1>Tasks</h1>
        <ul>
            <li v-for="task in tasks" :key="task.id">
                {{ task.title }} - {{ task.status }}
                <button @click="editTask(task)">Edit</button>
                <button @click="deleteTask(task.id)">Delete</button>
            </li>
        </ul>
        <form @submit.prevent="createTask">
            <input v-model="newTask.title" placeholder="Task title" required>
            <input v-model="newTask.description" placeholder="Task description">
            <button type="submit">Add Task</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tasks: [],
            newTask: {
                title: '',
                description: ''
            }
        };
    },
    methods: {
        fetchTasks() {
            axios.get('/api/tasks').then(response => {
                this.tasks = response.data;
            });
        },
        createTask() {
            axios.post('/api/tasks', this.newTask).then(response => {
                this.tasks.push(response.data);
                this.newTask.title = '';
                this.newTask.description = '';
            });
        },
        editTask(task) {
            // Логика редактирования
        },
        deleteTask(id) {
            axios.delete(`/api/tasks/${id}`).then(() => {
                this.tasks = this.tasks.filter(task => task.id !== id);
            });
        }
    },
    mounted() {
        this.fetchTasks();
    }
};
</script>
