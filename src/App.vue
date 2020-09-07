<template>
    <img alt="Vue logo" src="./assets/logo.png"/>
    <Todo msg="Hello Vue 3.0 + Vite"/>
</template>

<script>
    import Todo from './components/Todo.vue'

    export default {
        name: 'App',
        components: {
            Todo
        }
    }
</script>
