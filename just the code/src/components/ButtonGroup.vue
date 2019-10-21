<template>
    <div>
        <button type="button"
                class="btn btn-secondary btn-sm"
                @click="updateTask(task)">
                Update
        </button>
        &nbsp;
        <button type="button"
                class="btn btn-danger btn-sm"
                @click="deleteTask(task)">
                X
        </button>
    </div>
</template>
<script>
export default {
    name: 'ButtonGroup',
    props: ['task'],
    methods: {
        updateTask(task){
            this.$emit('updating', task)
        },
        deleteTask(task){
            this.$emit('deleting', task) 
        }
    }
}
</script>