<template>
  <div class="vstack gap-3 mt-4">
      <form id="new-todo-form" @submit.prevent="addTodo" class="form-group container">
        <input 
		  		type="text" 
		  		name="content" 
		  		id="content" 
		  		placeholder="Введите текст"
          class="form-control"
		  		v-model="input_content" 
        />
        <span>Приоритет</span>
				<div class="d-flex justify-content-between">
					<label>
						<input 
							type="radio" 
							name="category" 
							id="category1" 
							value="high"
							v-model="input_priority" />
						<div>Высокий</div>
					</label>

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category2" 
							value="low"
							v-model="input_priority" />
						<div>Низкий</div>
					</label>
				</div>
        <select v-model="input_category" class="form-select">
          <option disabled value="">Выберите категорию</option>
          <option value="work">Работа</option>
          <option value="personal">Личное</option>
          <option value="shopping">Покупки</option>
          <option value="other">Другое</option>
        </select>
        <input type="submit" value="Добавить" class="btn btn-primary mt-2" />
      </form>
      <div v-for="todo in todos" class="container">
        <input type="checkbox" v-model="todo.done" />
        <span class="category">({{ getCategoryName(todo.category) }})</span>
        <span :class="`${todo.priority === 'high' ? 'text-danger' : 'text-primary'}`">{{ todo.content }}</span>
		  </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const todos = ref([])
const input_content = ref('')
const input_category = ref(null)
const input_priority = ref(null)

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

const addTodo = () => {
  console.log(input_content.value)
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}

	todos.value.unshift({
		content: input_content.value,
		category: input_category.value,
		priority: input_priority.value,
		done: false,
		createdAt: new Date().getTime()
	})

  input_content.value = '';
  input_priority.value = null;
  input_category.value = null;
}

const getCategoryName = (category) => {
  switch (category) {
    case 'work':
      return 'Работа';
    case 'personal':
      return 'Личное';
    case 'shopping':
      return 'Покупки';
    case 'other':
      return 'Другое';
    default:
      return category;
  }
};

</script>

<style>
  .container {
    max-width: 400px;
  }
</style>