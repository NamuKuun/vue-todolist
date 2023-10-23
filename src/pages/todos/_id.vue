<template>
  <h1>To-Do Page</h1>
  <form>
    <div class="form-group">
      <label>Todo Subject</label>
      <input type="text" class="form-control" />
    </div>
    <button class="btn btn-primary mt-2">Save</button>
  </form>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
  setup() {
    const route = useRoute();

    console.log(route.params.id);
  },
};
</script>

<style></style>
