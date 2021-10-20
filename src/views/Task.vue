<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h1>{{ task.title }}</h1>
      <form @submit.prevent="submitHeandler">
        <div class="chips" ref="chips"></div>

        <div class="input-field">
          <textarea
            v-model="description"
            id="description"
            class="materialize-textarea"
          ></textarea>
          <label for="description">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px"
            >{{ description.length }}/2048</span
          >
        </div>

        <input type="text" ref="datepicker" />

        <div style="display: flex; justify-content: space-between">
          <div style="margin-bottom: 1rem" v-if="task.status !== 'completed'">
            <button class="btn" type="submit">Update</button>
            <button
              class="btn blue"
              type="button"
              style="margin-left: 1rem"
              @click="completeTask"
            >
              Complete Task
            </button>
          </div>
          <button class="btn red" type="button" @click="deleteTask">
            Delete
          </button>
        </div>
      </form>
    </div>
    <p v-else>Task not found</p>
  </div>
</template>

<script>
export default {
  computed: {
    task() {
      return this.$store.getters.taskById(+this.$route.params.id);
    },
  },
  data: () => ({
    description: "",
    chips: null,
    date: null,
  }),
  mounted() {
    this.description = this.task.description;
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: "Task tags",
      data: this.task.tags,
    });
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(this.task.date),
      setDefaultDate: true,
    });
    setTimeout(() => {
      M.updateTextFields();
    }, 0);
  },
  methods: {
    submitHeandler() {
      this.$store.dispatch("updateTask", {
        id: this.task.id,
        description: this.description,
        date: this.date.date,
      });
      this.$router.push("/list");
    },
    completeTask() {
      this.$store.dispatch("completeTask", this.task.id);
      this.$router.push("/list");
    },
    deleteTask() {
      this.$store.dispatch("deleteTask");
      this.$router.push("/list");
    },
  },
  destroyed() {
    if (this.date && this.date.destroy) {
      this.date.destroy();
    }
    if (this.chips && this.chips.destroy) {
      this.chips.destroy();
    }
  },
};
</script>

<style scoped>
textarea {
  min-height: 150px;
}
</style>