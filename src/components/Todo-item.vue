<template>
    <div class="flex items-center justify-between bg-white text-[#242424] p-4 rounded-lg shadow-md my-8">
        <div class="flex gap-2">
            <div class="flex items-center">
                <img 
                    src="../assets/checked.png" 
                    v-if='task.status === "done"' 
                    alt="Checked" 
                    @click="toogleStatus" 
                    class="w-4 h-4 cursor-pointer">
                <input 
                    v-else type="radio" 
                    class="cursor-pointer" 
                    @click="toogleStatus">
            </div>

            <div v-if="isEditing">
                <input 
                v-model="editedText"
                @keyup.enter="saveEditedTask" 
                ref="editInput"
                class="border border-gray-300 rounded px-2 py-1"/>
            </div>
            <p v-else :class="{ 'line-through text-gray-800': task.status === 'done' }">
                {{ task.text }}
            </p>
        </div>
        
        <div class="flex gap-2">
            <button @click="editTask" v-if='task.status !== "done"'>
                <img src="../assets/edit.png" alt="Edit" class="w-5 h-5 cursor-pointer">
            </button>
            <button @click="deleteTask">
                <img src="../assets/delete.png" alt="Delete" class="w-5 h-5 cursor-pointer">
            </button>
        </div>
    </div>
</template>

<script>
import { nextTick } from 'vue'

    export default {
        name: 'TodoItem',
        props: {
            task: {
                type: Object
            }
        },
        data() {
            return {
                isEditing: false,
                editedText: this.task.text,
            }
            
        },
        methods: {
            deleteTask() {
                // emit the task id to the parent
                this.$emit("delete", this.task.id)
            },

            async editTask() {
                this.isEditing = true
                await nextTick()
                this.$refs.editInput.focus()
                
            },
            saveEditedTask() {
                this.isEditing = false
                this.$emit("edit", {
                    id: this.task.id,
                    text: this.editedText.trim()
                })
            },

            toogleStatus() {
                const newStatus = this.task.status === "done" ? "to do" : "done"
                this.$emit("toggleStatus", {
                    id: this.task.id,
                    status: newStatus
                })
            }
        }
    }
</script>
