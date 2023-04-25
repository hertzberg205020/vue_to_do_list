<template>
	<div class="todo-header">
		<input
			type="text"
			placeholder="請輸任務名稱，按下Enter鍵確定"
			@keydown.enter="add" />
	</div>
</template>

<script>
	import { nanoid } from 'nanoid';
	export default {
		name: 'ToDoHeader',
		props: ['addToDoItem'],
		methods: {
			add(evt) {
				if (evt.target.value.trim().length === 0)
					return alter('輸入不可為空');
				const item = {
					id: nanoid(),
					title: evt.target.value,
					done: false,
				};
				this.addToDoItem(item);
				evt.target.value = '';
			},
		},
	};
</script>

<style scoped>
	/*header*/
	.todo-header input {
		width: 560px;
		height: 28px;
		font-size: 14px;
		border: 1px solid #ccc;
		border-radius: 4px;
		padding: 4px 7px;
	}

	.todo-header input:focus {
		outline: none;
		border-color: rgba(82, 168, 236, 0.8);
		box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
			0 0 8px rgba(82, 168, 236, 0.6);
	}
</style>
