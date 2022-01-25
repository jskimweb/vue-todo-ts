<template>
	<div>
		<label for="todo-input">오늘 할 일 : </label>
		<input id="todo-input" type="text" :value="item" @input="handleInput">
		<button @click="addTodo" type="button">추가</button>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
	props: {
		item: {
			type: String,
			required: true
		}
	},
	setup (props, { emit }) {
		const handleInput = (event: InputEvent) => {
			const eventTarget = event.target as HTMLInputElement;	
			emit('input', eventTarget.value);
		}
		const addTodo = () => {
			emit('add');
		}		

		return {
			handleInput,
			addTodo
		}
	}
})
</script>

<style scoped>

</style>