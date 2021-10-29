<template>
    <AddTask v-show="showAddTaskForm" 
    @add-task="addTask" />
    <Tasks @toggle-reminder="toggleReminder" 
           @delete-task="deleteTask" 
           :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
    name: 'Home',
    props: {
        showAddTaskForm: Boolean,
    },
    components: {
        Tasks,
        AddTask
    },
    data() {
       return {
            tasks: [],
       } 
    },
    methods: {
        async addTask(newtask) {
            const res = await fetch('api/tasks', {
            method: 'POST',
            headers: {
                'Content-type': 'application/json',
            },
            body: JSON.stringify(newtask)
         })

        const data = await res.json()

        this.tasks = [...this.tasks, data]
        },
        async deleteTask(id){
            //console.log('task', id);
            if( confirm('Are you sure to delete this task?')){
                const res = await fetch( `api/tasks/${id}`, {
                method: 'DELETE'
                })

                res.status === 200 
                ? ( this.tasks = this.tasks.filter( 
                (task)=> task.id !== id ) ) 
                : alert('Error in deleting task')
            }
         },
        async toggleReminder(id){
            //console.log(id);
            const taskToToggle = await this.fetchTask(id)
            const updTask = {...taskToToggle, reminder: !taskToToggle.reminder }
            
            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: {
                'Content-type': 'application/json'
                },
                body: JSON.stringify(updTask)
            })
            const data = await res.json()
            this.tasks = this.tasks.map( (task) => 
                task.id === id ? {...task, reminder: data.reminder } : task
            );
        },
        async fetchTasks() {
            const res = await fetch('api/tasks')

            const data = await res.json()

            return data
            },
            async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`)

            const data = await res.json()

            return data
            }
        },
        async created() {
            this.tasks = await this.fetchTasks() 
        }
    }
</script>