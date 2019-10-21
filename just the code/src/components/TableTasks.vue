<template>
<table class="table table-dark table-stripped table-hover">
    <thead class="thead-light">
        <tr>
            <th>#</th>
            <th>Your tasks</th>
            <th>Completed?</th>
            <th>Change</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(task, index) in toDoList" :key="task.id">
            <td>{{ index+1 }}</td>
            <td>{{ task.title }}</td>
            <td v-if="task.is_completed">Done!</td>
            <td v-else>Yet to be done...</td>
            <td><ButtonGroup :task='task' @updating='emitUpdateUp' @deleting='emitDeleteUp'/></td>
        </tr>
    </tbody>
</table>
</template>

<script>
import ButtonGroup from './ButtonGroup.vue'
export default {
    name: 'TableTasks',
    props: ['toDoList'],
    methods:{
        emitUpdateUp(task){
            this.$emit('requestForUpdate', task)
        },
        emitDeleteUp(task){
            this.$emit('deleting', task)
        }
    },
    components: {
        ButtonGroup
    }     
    
}
</script>