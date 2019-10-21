<template>
    <input type="button" value="Add a task" class="btn btn-info" @click="$emit('newTask')">
</template>

<script>
export default {
    name: 'AddTask',
}
</script>
