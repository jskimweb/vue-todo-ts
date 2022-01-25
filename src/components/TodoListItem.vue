<template>
	<li>
		<span
		class="item"
		@click="toggleItem"
		:class="todoItemClass"
		>{{ todoItem.title }}</span>
		<button @click="removeItem">삭제</button>
	</li>
</template>

<script lang="ts">
import { Todo } from '@/App.vue';
import { defineComponent, PropType, computed } from 'vue'

export default defineComponent({
	props: {
		todoItem: Object as PropType<Todo>,
		index: Number
	},
	setup (props, { emit }) {
		const todoItemClass = computed((): string | null => {
			return props.todoItem?.done ? 'complete' : null;
		});

		const toggleItem = () => {
			emit('toggle', props.todoItem, props.index);
		};
		const removeItem = () => {
			emit('remove', props.index);
		};

		return {
			todoItemClass,
			toggleItem,
			removeItem
		}
	}
})
</script>

<style scoped>
	.item {
		cursor: pointer;
	}

	.complete {
		text-decoration: line-through;
	}
</style>