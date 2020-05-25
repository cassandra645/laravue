<template>
    <div class="container">
        <add-task @task-added="refresh"></add-task>
        <ul class="list-group">
            <li class="list-group-item d-flex justify-content-between align-items-center" v-for="task in tasks.data" :key="task.id"><a href="#">{{ task.name }}</a>
                <div>
                    <button type="button" class="btn btn-secondary" data-toggle="modal" data-target="#editModal" @click="getTask(task.id)">Éditer</button>
                    <button type="button" class="btn btn-danger" @click="deleteTask(task.id)">Supprimer</button>
                </div>
            </li>
            <edit-task v-bind:taskToEdit="taskToEdit" @task-updated="refresh"></edit-task>
        </ul>
        <pagination :data="tasks" @pagination-change-page="getResults" class="mt-5"></pagination>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                tasks: {},
                taskToEdit: ''
            }
        },

        created() {
            axios.get('http://laravue.test/tasksList')
                .then(response => this.tasks = response.data)
                .catch(error => console.log(error));
        },

        methods: {
            getResults(page = 1) {
			    axios.get('http://laravue.test/tasksList?page=' + page)
			    	.then(response => {
			    		this.tasks = response.data;
                    });
            },

            getTask(id) {
                axios.get('http://laravue.test/tasks/edit/' + id)
                    .then(response => this.taskToEdit = response.data)
                    .catch(error => console.log(error));
            },

            deleteTask(id) {
                axios.delete('http://laravue.test/tasks/' + id)
                    .then(response => this.tasks = response.data)
                    .catch(error => console.log(error));
            },

            refresh(tasks) {
                this.tasks = tasks.data
            }
        },

        mounted() {
            console.log('Component mounted.')
        }
    }
</script>