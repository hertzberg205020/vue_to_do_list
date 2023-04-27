<template>
	<div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
				<ToDoHeader @addToDoItem="addToDoItem" />
				<ToDoList :toDoList="toDoList" />
				<ToDoFooter
					:toDoList="toDoList"
					@checkAllToDoItem="checkAllToDoItem"
					@clearAllDoneToDoItem="clearAllDoneToDoItem" />
			</div>
		</div>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js';

	import ToDoHeader from './components/ToDoHeader.vue';
	import ToDoList from './components/ToDoList.vue';
	import ToDoFooter from './components/ToDoFooter.vue';

	export default {
		name: 'App',
		components: {
			ToDoHeader,
			ToDoList,
			ToDoFooter,
		},
		data() {
			return {
				toDoList:
					JSON.parse(window.localStorage.getItem('toDoList')) || [],
			};
		},
		methods: {
			// 數據在哪裡，操作數據的方法就在哪裡
			addToDoItem(item) {
				this.toDoList.unshift(item);
			},
			checkToDoItem(id) {
				// this.toDoList.forEach(item => {
				// 	if (item.id === id) {
				// 		item.done = !item.done;
				// 	}
				// });
				const target = this.toDoList.find(item => item.id === id);

				target.done = !target.done;
			},
			deleteToDoItem(id) {
				const index = this.toDoList.findIndex(item => item.id === id);

				this.toDoList.splice(index, 1);
			},

			checkAllToDoItem(done) {
				this.toDoList.forEach(item => {
					item.done = done;
				});
			},

			clearAllDoneToDoItem() {
				this.toDoList = this.toDoList.filter(item => !item.done);
			},
			updateToDoItem({ id, title }) {
				const target = this.toDoList.find(item => item.id === id);
				target.title = title;
				target.isEditing = false;
			},
		},
		watch: {
			toDoList: {
				handler(newVal, oldVal) {
					window.localStorage.setItem(
						'toDoList',
						JSON.stringify(newVal)
					);
				},
				deep: true,
			},
		},
		mounted() {
			this.$bus.$on('checkToDoItem', this.checkToDoItem);
			// this.$bus.$on('deleteToDoItem', this.deleteToDoItem);
			this.pubId = pubsub.subscribe('deleteToDoItem', (_, id) => {
				this.deleteToDoItem(id);
			});
			this.$bus.$on('editToDoItem', this.updateToDoItem);
		},
		beforeDestroy() {
			this.$bus.$off('checkToDoItem', this.checkToDoItem);
			this.$bus.$off('editToDoItem', this.updateToDoItem);
			pubsub.unsubscribe(this.pubId);
		},
	};
</script>

<style>
	/*base*/
	body {
		background: #fff;
	}

	.btn {
		display: inline-block;
		padding: 4px 12px;
		margin-bottom: 0;
		font-size: 14px;
		line-height: 20px;
		text-align: center;
		vertical-align: middle;
		cursor: pointer;
		box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
			0 1px 2px rgba(0, 0, 0, 0.05);
		border-radius: 4px;
	}

	.btn-danger {
		color: #fff;
		background-color: #da4f49;
		border: 1px solid #bd362f;
	}

	.btn-edit {
		color: #fff;
		background-color: skyblue;
		border: 1px solid rgb(103, 159, 180);
		margin-right: 5px;
	}

	.btn-danger:hover {
		color: #fff;
		background-color: #bd362f;
	}

	.btn-edit:hover {
		color: #fff;
		background-color: rgb(161, 213, 234);
	}

	.btn:focus {
		outline: none;
	}

	.todo-container {
		width: 600px;
		margin: 0 auto;
	}
	.todo-container .todo-wrap {
		padding: 10px;
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>
