<template>
    <div class="flex flex-col w-[500px] min-h-[600px] bg-gray-200 rounded-4xl shadow-lg p-12 relative">
        <h2 class="font-semibold text-[#242424] text-2xl">My Todo App</h2>
        <div class="flex flex-col mt-4">
            <form class="relative" @submit.prevent="addTask">
                <input type="text" v-model="task" class="w-full border-2 text-[#242424] border-gray-300 rounded-lg p-2 my-4" placeholder="Add a new task..."/>
                <button>
                    <img src="../assets/add.png" alt="Add" class="absolute right-5 top-7 w-5 h-5 cursor-pointer"/>
                </button>
            </form>
            
            <ul class="mb-8">
                <li 
                    v-for="(task, index) in tasks" 
                    :key="index" >
                        <TodoItem 
                            :task= "task"
                            @delete="deleteTask"
                            @edit="editTask"
                            @toggleStatus="toggleTaskStatus"
                        />
                </li>
            </ul>
        </div>
        <div class="text-[#242424] text-left py-12 absolute bottom-0">
            <p>Pending Tasks: {{ tasks.filter(task => task.status === "to do").length }}</p>
        </div>
    </div>
</template>

<script>
import TodoItem from './Todo-item.vue';

    export default {
        name: 'TodoList',
        components: {
            TodoItem
        },
        data() {
            return {
                tasks: JSON.parse(localStorage.getItem("tasks")) || [],
                task: ""
            };
        },
        methods: {
            addTask() {

                if (this.task.length === 0) return

                this.tasks.push({
                    id: Date.now(),
                    text: this.task,
                    status: "to do"
                })

                localStorage.setItem("tasks", JSON.stringify(this.tasks))

                this.task = ""
            },

            deleteTask(id) {
                this.tasks = this.tasks.filter(task => task.id !== id)
                localStorage.setItem("tasks", JSON.stringify(this.tasks))
            },

            editTask(updatedTask) {
                const foundTask = this.tasks.find(task => task.id === updatedTask.id)
                if (foundTask) {
                    foundTask.text = updatedTask.text
                    localStorage.setItem("tasks", JSON.stringify(this.tasks))
                } 
            },
            toggleTaskStatus(updated) {
                const foundTask = this.tasks.find(task => task.id === updated.id)
                if (foundTask) {
                    foundTask.status = updated.status
                    localStorage.setItem("tasks", JSON.stringify(this.tasks))
                }
            }
        }

    }
</script>