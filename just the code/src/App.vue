<template>
  <div id="app" class="card center_div">
    <Alert
      :alertMessage="alertMessage"
      :showDismissibleAlert="showDismissibleAlert"
      @alertChanged="alertChanged"
    />
    <b-modal v-model="modalShow" title="Add/update tasks" hide-footer>
      <Modal :task="task" :updateOrAdd="updateOrAdd" @updating="addNewInfo" @addNew="addingNew" />
    </b-modal>
    <Header />
    <NumberOfTasks :toDoList='toDoList'/>
    <AddTask @newTask="newTask" />
    <TableTasks :toDoList="toDoList" @requestForUpdate="updateMethod" @deleting="deleteMethod" />
  </div>
</template>

<script>
import uuid from 'uuid/v4';
import Header from "./components/Header";
import AddTask from "./components/AddTask";
import TableTasks from "./components/TableTasks";
import Modal from "./components/Modal";
import Alert from "./components/Alert";
import NumberOfTasks from "./components/NumberOfTasks";

export default {
  name: "app",
  data() {
    return {
      toDoList: [],
      task: {},
      modalShow: false,
      updateOrAdd: false,
      alertMessage: "",
      showDismissibleAlert: false
    };
  },
  methods: {
    updateMethod(task) {
      this.updateOrAdd = true;
      this.task = task;
      this.modalShow = true;
    },
    deleteMethod(task) {
      for (let x of this.toDoList) {
        x.title == task.title
          ? this.toDoList.splice(this.toDoList.indexOf(x), 1)
          : null;
      }
      this.alertMessage = "The task has been successfully deleted!";
      this.showDismissibleAlert = true;
    },
    addNewInfo(task) {
      this.toDoList.forEach(e => {
        e.id == task.id ? (e.title = task.title, e.is_completed = task.is_completed) : null
      });
      this.modalShow = false;
      this.alertMessage = "The task has been successfully updated!";
      this.showDismissibleAlert = true;
    },
    newTask() {
      this.updateOrAdd = false;
      this.modalShow = true;
    },
    addingNew(task) {
      let newTask = {};
      newTask.title = task.title;
      newTask.is_completed = task.is_completed;
      newTask.id = uuid();
      this.toDoList.push(newTask);
      this.modalShow = false;
      this.alertMessage = "The task has been successfully added!";
      this.showDismissibleAlert = true;
    },
    alertChanged() {
      this.showDismissibleAlert = !this.showDismissibleAlert
    }
  },
  watch: {
    toDoList: {
      handler() {
        localStorage.setItem("toDoList", JSON.stringify(this.toDoList));
      },
      deep: true
    }
  },
  mounted() {
    if (localStorage.getItem("toDoList"))
      this.toDoList = JSON.parse(localStorage.getItem("toDoList"));
  },
  components: {
    Header,
    AddTask,
    TableTasks,
    Modal,
    Alert,
    NumberOfTasks
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 50px;
}
.center_div {
  margin: 0 auto;
  width: 650px;
}
</style>
