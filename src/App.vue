<template>
  <div class="container">
    <h2>To-Do-List</h2>
    <div class="flex-grow-1 mr-2">
      <input
        class="form-control"
        type="text"
        v-model="searchText"
        placeholder="Search"
        @keyup.enter="searchTodo"
      />
    </div>
    <hr />
    <TodoSimpleForm @add-todo="addTodo" />
    <div style="color: red">{{ error }}</div>
    <div v-if="!todos.length">
      there is nothing to display
    </div>

    <!-- :todos 바인딩으로 자식 컴포넌트인 TodoList에 todos를 넘겨준다 -->
    <TodoList
      :todos="todos"
      @toggle-todo="toggletodo"
      @delete-todo="deletetodo"
    />

    <!-- pagination 설정(BootStrap에서 복사) -->
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li v-if="currentPage !== 1" class="page-item">
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage - 1)"
            >Previous</a
          >
        </li>

        <li
          v-for="page in numberOFPages"
          :key="page"
          class="page-item"
          :class="currentPage === page ? 'active' : ''"
        >
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(page)"
            >{{ page }}</a
          >
        </li>
        <li
          v-if="currentPage !== numberOFPages"
          class="page-item"
        >
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage + 1)"
            >Next</a
          >
        </li>
      </ul>
    </nav>
  </div>
</template>
<script>
// 컴포넌트 불러오기 .. 객체 명은 내 마음대로 작성가능하지만, 가능하면 vue파일 이름으로 통일하자
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import { ref, computed, watch } from 'vue';
import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },

  setup() {
    const todos = ref([]);
    const numberOFTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);
    const error = ref('');
    const searchText = ref('');

    // math.ceil <<== 올림 함수 .. // Todos의 총합을 limit으로 나눈뒤 반올림해서 페이지를 설정한다
    const numberOFPages = computed(() => {
      return Math.ceil(numberOFTodos.value / limit);
    });

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gary',
    };

    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;
      try {
        const res = await axios.get(
          `http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`
        );
        numberOFTodos.value = res.headers['x-total-count'];
        todos.value = res.data;
      } catch (err) {
        error.value = 'Something went wrong.';
      }
    };

    getTodos();

    // TodoSimpleForm컴포넌트에서 emit해준 객체를 파라메터로 해서 todos배열에 추가해준다
    const addTodo = async (todo) => {
      // datebase에 todo를 추가하기
      try {
        await axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed,
        });
        getTodos(1);
      } catch (err) {
        error.value = 'Something went wrong.';
      }
    };
    const deletetodo = async (index) => {
      error.value = '';
      // 받아온 인덱스를 splice함수를 이용해 삭제해준다.
      const id = todos.value[index].id;
      try {
        await axios.delete(
          'http://localhost:3000/todos/' + id
        );
        todos.value.splice(index, 1);
      } catch (err) {
        error.value = 'Something went wrong.';
      }
    };

    const toggletodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        await axios.patch(
          'http://localhost:3000/todos/' + id,
          {
            completed: !todos.value[index].completed,
          }
        );
        todos.value[index].completed =
          !todos.value[index].completed;
      } catch (err) {
        error.value = 'Something went wrong.';
      }
    };

    // const filteredTodo = computed(() => {
    //   if (searchText.value) {
    //     return todos.value.filter((todo) => {
    //       return todo.subject.includes(searchText.value);
    //     });
    //   }

    //   return todos.value;
    // });
    let timeout = null;
    const searchTodo = () => {
      clearTimeout(timeout);
      getTodos(1);
    };

    watch(searchText, () => {
      clearTimeout(timeout);
      timeout = setTimeout(() => {
        getTodos(1);
      }, 2000);
    });

    return {
      todoStyle,
      addTodo,
      todos,
      deletetodo,
      toggletodo,
      // filteredTodo,
      searchText,
      error,
      numberOFPages,
      currentPage,
      getTodos,
      searchTodo,
    };
  },
};
</script>

<style>
.Error {
  color: red;
}
</style>
