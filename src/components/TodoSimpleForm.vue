<template>
  <form @submit.prevent="onSubmit">
    <div class="d-flex">
      <div class="flex-grow-1 mr-2">
        <input
          class="form-control"
          type="text"
          v-model="todo"
          placeholder="type new to-do"
        />
      </div>

      <div>
        <button class="btn btn-primary" type="submit">
          Add
        </button>
      </div>
    </div>

    <div class="Error" v-show="hasError">
      This field cannot be empty
    </div>
  </form>
</template>

<script>
import { ref } from 'vue';
export default {
  // vue3부터는 emits에도 배열을 넣어서 보내야한다.
  emits: ['add-todo'],

  setup(props, { emit }) {
    const todo = ref('');

    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      } else {
        emit('add-todo', {
          id: Date.now,
          subject: todo.value,
          completed: false,
        });
        hasError.value = false;
        todo.value = '';
      }
    };
    const hasError = ref(false);

    return {
      todo,
      onSubmit,
      hasError,
    };
  },
};
</script>

<style></style>
