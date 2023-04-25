<template>
	<ul class="todo-main">
		<ToDoItem
			v-for="item in toDoList"
			:key="item.id"
			:item="item" />
	</ul>
</template>

<script>
	import ToDoItem from './ToDoItem.vue';
	export default {
		name: 'ToDoList',
		components: {
			ToDoItem,
		},
		props: ['toDoList'],
	};
</script>

<style scoped>
	/*main*/
	.todo-main {
		margin-left: 0px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding: 0px;
	}

	.todo-empty {
		height: 40px;
		line-height: 40px;
		border: 1px solid #ddd;
		border-radius: 2px;
		padding-left: 5px;
		margin-top: 10px;
	}
</style>
