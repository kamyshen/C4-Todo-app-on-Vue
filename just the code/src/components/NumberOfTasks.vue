<template>
  <div>
    <p>There are {{ total }} tasks on the list, from which {{ doneTasks }} have been completed</p>
  </div>
</template>
<script>
export default {
  name: "NumberOfTasks",
  props: ["toDoList"],
  computed: {
    total() {
      return this.toDoList.length;
    },
    doneTasks() {
      let d = 0;
      for (let task of this.toDoList) {
        if (task.is_completed == true) d++;
      }
      return d;
    }
  }
};
</script>