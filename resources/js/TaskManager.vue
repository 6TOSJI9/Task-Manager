<template>
    <div class="container mt-4">
        <h1>Список задач</h1>
        <form @submit.prevent="createTask">
            <div class="mb-3">
                <label for="title" class="form-label">Название задачи</label>
                <input type="text" v-model="newTask.title" class="form-control" id="title" required>
            </div>
            <div class="mb-3">
                <label for="description" class="form-label">Описание</label>
                <textarea v-model="newTask.description" class="form-control" id="description"></textarea>
            </div>
            <div class="mb-3">
                <label for="priority" class="form-label">Приоритет</label>
                <select v-model="newTask.priority" class="form-select" id="priority" required>
                    <option value="low">Низкий</option>
                    <option value="medium">Средний</option>
                    <option value="high">Высокий</option>
                </select>
            </div>
            <button type="submit" class="btn btn-primary">Добавить задачу</button>
        </form>

        <ul class="list-group mt-3">
            <li v-for="task in tasks" :key="task.id" class="list-group-item">
                <h5>{{ task.title }}</h5>
                <p>{{ task.description }}</p>
                <p>Приоритет: {{ task.priority }}</p>
                <p>Статус: {{ task.status }}</p>
                <button class="btn btn-danger" @click="deleteTask(task.id)">Удалить</button>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            tasks: [],
            newTask: {
                title: '',
                description: '',
                priority: 'medium',
                status: 'pending',
                user_id: 1, // Укажи ID пользователя, который создает задачу
            },
        };
    },
    methods: {
        fetchTasks() {
            axios.get('/api/tasks')
                .then(response => {
                    this.tasks = response.data;
                })
                .catch(error => {
                    console.error('Error fetching tasks:', error);
                });
        },
        createTask() {
            axios.post('/api/tasks', this.newTask)
                .then(response => {
                    this.tasks.push(response.data); // Добавляем новую задачу в список
                    this.newTask = { title: '', description: '', priority: 'medium', status: 'pending', user_id: 1 }; // Сброс формы
                })
                .catch(error => {
                    console.error('Error creating task:', error);
                });
        },
        deleteTask(taskId) {
            axios.delete(`/api/tasks/${taskId}`)
                .then(() => {
                    this.tasks = this.tasks.filter(task => task.id !== taskId);
                })
                .catch(error => {
                    console.error('Error deleting task:', error);
                });
        },
    },
    mounted() {
        this.fetchTasks();
    },
};
</script>

<style scoped>
/* Добавь свои стили, если нужно */
</style>
