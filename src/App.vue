<template>
  <div class="container">
    <h2>To-Do-List</h2>
    <TodoSimpleForm @add-todo="addTodo" />
    <div v-if="!todos.length">추가된 Todo가 없습니다</div>
    <!-- :todos 바인딩으로 자식 컴포넌트인 TodoList에 todos를 넘겨준다 -->
    <TodoList
      :todos="todos"
      @toggle-todo="toggletodo"
      @delete-todo="deletetodo"
    />
  </div>
</template>
<script>
// 컴포넌트 불러오기 .. 객체 명은 내 마음대로 작성가능하지만, 가능하면 vue파일 이름으로 통일하자
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import { ref } from 'vue';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },

  setup() {
    const todos = ref([]);

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gary',
    };

    // TodoSimpleForm컴포넌트에서 emit해준 객체를 파라메터로 해서 todos배열에 추가해준다
    const addTodo = (todo) => {
      console.log(todo);
      todos.value.push(todo);
    };
    const deletetodo = (index) => {
      // 받아온 인덱스를 splice함수를 이용해 삭제해준다.
      todos.value.splice(index, 1);
    };

    const toggletodo = (index) => {
      console.log(index);
    };
    return {
      todoStyle,
      addTodo,
      todos,
      deletetodo,
      toggletodo,
    };
  },
};
</script>

<style>
.Error {
  color: red;
}
</style>
