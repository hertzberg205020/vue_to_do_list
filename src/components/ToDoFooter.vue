<template>
	<div
		class="todo-footer"
		v-show="total">
		<label>
			<input
				type="checkbox"
				v-model="isAll" />
		</label>
		<span>
			<span>已完成{{ doneCount }}</span> / 全部{{ total }}
		</span>
		<button
			class="btn btn-danger"
			@click="clearAllDone">
			清除已完成任務
		</button>
	</div>
</template>

<script>
	export default {
		name: 'ToDoFooter',
		// props: ['toDoList', 'checkAllToDoItem', 'clearAllDoneToDoItem'],
		props: ['toDoList'],
		computed: {
			// 已完成的任務數量
			doneCount() {
				// return this.toDoList.filter(item => item.done).length;
				return this.toDoList.reduce((acc, curVal) => {
					return acc + (curVal.done ? 1 : 0);
				}, 0);
				// reduce(callback, initialValue)
				// reduce中的callback第一次執行時，accumulator的值為initialValue
				// 之後都是上一次callback的return值
				// callback: (accumulator, currentValue, currentIndex, array) => { ... }
			},
			total() {
				return this.toDoList.length;
			},
			isAll: {
				get() {
					return this.doneCount === this.total && this.total > 0;
				},

				set(val) {
					// 將toDoList中的所有toDoItem的done屬性值設為val
					// this.checkAllToDoItem(val);
					this.$emit('checkAllToDoItem', val);
				},
			},
		},
		methods: {
			clearAllDone() {
				// 將toDoList中的所有已完成的toDoItem刪除
				// this.clearAllDoneToDoItem();
				this.$emit('clearAllDoneToDoItem');
			},
		},
	};
</script>

<style scoped>
	/*footer*/
	.todo-footer {
		height: 40px;
		line-height: 40px;
		padding-left: 6px;
		margin-top: 5px;
	}

	.todo-footer label {
		display: inline-block;
		margin-right: 20px;
		cursor: pointer;
	}

	.todo-footer label input {
		position: relative;
		top: -1px;
		vertical-align: middle;
		margin-right: 5px;
	}

	.todo-footer button {
		float: right;
		margin-top: 5px;
	}
</style>
