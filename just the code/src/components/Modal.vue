<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset" class="w-100">
      <b-form-group id="form-description-group">
        <div>
          <b-form-input
            type="text"
            required
            :state="validation"
            placeholder="Your task"
            v-model="tempTitle"
          ></b-form-input>
          <b-form-invalid-feedback :state="validation">Enter something real!</b-form-invalid-feedback>
          <b-form-valid-feedback :state="validation">Looks good.</b-form-valid-feedback>
        </div>
      </b-form-group>
      <b-form-group id="form-complete-group">
        <b-form-checkbox value="true" v-model="checked">Is the task done?</b-form-checkbox>
      </b-form-group>
      <b-button type="submit" variant="primary" class="float-right">Submit</b-button>
      <b-button type="reset" variant="danger" class="float-right mr-3">Reset</b-button>
    </b-form>
  </div>
</template>
<script>
export default {
  name: "Modal",
  props: ["task", "updateOrAdd"],
  data() {
    return {
      tempTitle: "",
      checked: []
    };
  },
  computed: {
    validation() {
      return this.tempTitle.length > 3 && this.tempTitle.length < 30;
    }
  },
  created() {
    if (this.updateOrAdd) {
      this.tempTitle = this.task.title;
      // d.id = this.task.id;
      this.task.is_completed ? (this.checked = ["true"]) : (this.checked = []);
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      let form = {};
      if (this.tempTitle.length < 4 || this.tempTitle.length > 30) return;
      form.title = this.tempTitle;
      this.checked[0] == "true"
        ? (form.is_completed = true)
        : (form.is_completed = false);
      form.id = this.task.id;
      this.updateOrAdd
        ? this.$emit("updating", form)
        : this.$emit("addNew", form);
    },
    onReset(e) {
      e.preventDefault();
      if (this.updateOrAdd) {
        this.tempTitle = this.task.title;
        this.task.is_completed ? (this.checked = ["true"]) : this.checked.pop();
      } else {
        this.tempTitle = "";
        this.checked.pop();
      }
    }
  }
};
</script>