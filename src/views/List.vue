<template>
  <div>
    <h1>List</h1>
    <div class="row">
      <div class="input-field s6 col">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Choose your option</option>
          <option value="Active">Active</option>
          <option value="Outdated">Outdated</option>
          <option value="Complited3">Complited</option>
        </select>
        <label>Status filter</label>
      </div>
    </div>
    <hr />
    <table v-if="allTasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, idx) of allTasks" :key="task.id">
          <td>{{ idx + 1 }}</td>
          <td>{{ task.title }}</td>
          <td>{{ new Date(task.date).toLocaleDateString() }}</td>
          <td class="decrip_wrap">
            <div class="descrip">{{ task.description }}</div>
          </td>
          <td>{{ task.status }}</td>
          <td>
            <router-link
              tag="button"
              class="btn btn-small"
              :to="'/task/' + task.id"
              >Open
            </router-link>
          </td>
          <td>
            <button class="btn red btn-small" @click="deleteTask">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "List",
  data: () => ({
    filter: null,
  }),
  computed: mapGetters(["allTasks"]),
  methods: {
    deleteTask() {
      this.$store.dispatch("deleteTask");
    },
  },
  mounted() {
    M.FormSelect.init(this.$refs.select);
  },
};
</script>

<style scoped>
.title {
  font-size: 30px;
  font-weight: 700;
}
.decrip_wrap {
  max-width: 400px;
}
.descrip {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>
