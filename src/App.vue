<template>
    <div class="flex min-h-screen items-center justify-center">
        <TodoList />
    </div>
</template>

<script>
    import TodoList from './components/Todo-list.vue';

    export default {
        name: 'App',
        components: {
            TodoList
        }
    }
</script>
