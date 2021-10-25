<template>
    <header>
        <h1>{{title}}</h1>
        <Button btnText="Add Task" color="green"/>
        <Button btnText="Update Task" color="grey"/>
        <Button btnText="Delete Task" color="orange"/>
    </header>
</template>

<script>
import Button from './Button';

export default {
    name: 'Header',
    props: {
        title: {
            type: String,
            default: 'Task Tracker default'
        }
    },
    components: {
        Button
    }
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>