<template>
  <div>
    <div class="card mt-2" v-for="(t, index) in todos" :key="t.id">
      <div class="card-body p-2 d-flex align-items-center" style="cursor: pointer" @click="movetoPage(t.id)">
        <div class="form-check flex-grow-1">
          <input class="form-check-input" type="checkbox" :checked="t.completed" @click.stop="toggleTodo(index, $event)" />
          <!--t.completed가 참이면 todoStyle를 , 거짓이면 {}를 실행-->
          <label class="form-check-label" :class="{ todoSt: t.completed }">
            {{ t.subject }}
          </label>
        </div>
        <div>
          <button class="btn btn-danger btn-sm" @click.stop="deletetodo(index)">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { useRouter } from 'vue-router';
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  // vue3부터는 emits에도 배열을 넣어서 보내야한다.
  emits: ['toggle-todo', 'delete-todo'],

  setup(props, { emit }) {
    const router = useRouter();
    const toggleTodo = (index, event) => {
      emit('toggle-todo', index, event.target.checked);
    };

    const deletetodo = (index) => {
      emit('delete-todo', index);
    };

    const movetoPage = (todoid) => {
      router.push({
        name: 'Todo',
        params: {
          id: todoid,
        },
      });
    };
    return {
      toggleTodo,
      deletetodo,
      movetoPage,
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
