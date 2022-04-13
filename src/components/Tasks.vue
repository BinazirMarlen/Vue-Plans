<template>
    <div class="block"> 
        <div :key="task.id" class="task" v-for="task in tasks" >
        <Task @toggle-reminder="$emit('toggle-reminder', task.id)"
         @delete-task="$emit('delete-task', task.id)" :task="task"/>
        </div>
    </div> 
</template>

<script>
import Task from './Task.vue'
export default {
    name: 'Tasks',
    props: {
        tasks:{
            type:Array,
            required: true
        }
    },
    components: {
        Task
    },
    emits: ['delete-task', 'toggle-reminder']
}
</script>

<style>

</style>