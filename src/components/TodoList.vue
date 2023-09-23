<template>
  <div
    class="card mt-2"
    v-for="(t, index) in todos"
    :key="t.id"
  >
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check flex-grow-1">
        <input
          class="form-check-input"
          type="checkbox"
          :value="t.completed"
          @change="toggleTodo(index)"
        />
        <!--t.completed가 참이면 todoStyle를 , 거짓이면 {}를 실행-->
        <label
          class="form-check-label"
          :class="{ todoSt: t.completed }"
        >
          {{ t.subject }}
        </label>
      </div>
      <div>
        <button
          class="btn btn-danger btn-sm"
          @click="deletetodo(index)"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  setup(props, context) {
    const toggleTodo = (index) => {
      context.emit('toggle-todo', index);
    };

    const deletetodo = (index) => {
      context.emit('delete-todo', index);
    };
    return {
      toggleTodo,
      deletetodo,
    };
  },
};
</script>

<style>
.todoSt {
  color: gray;
  text-decoration: line-through;
}
</style>
