<template>
	<transition
		name="todo"
		appear>
		<li>
			<label>
				<input
					type="checkbox"
					:checked="item.done"
					@change="handleCheck(item.id)" />
				<span v-show="!item.isEditing">{{ item.title }}</span>
				<div
					v-show="item.isEditing"
					style="display: inline">
					<input
						type="text"
						ref="editInput"
						:value="item.title"
						@blur="handleBlur(item, $event)" />
				</div>
			</label>
			<button
				class="btn btn-danger"
				@click="handleDelete(item.id)">
				刪除
			</button>
			<button
				class="btn btn-edit"
				v-show="!item.isEditing"
				@click="handleEdit(item)">
				修改
			</button>
		</li>
	</transition>
</template>

<script>
	import pubsub from 'pubsub-js';

	export default {
		name: 'ToDoItem',
		props: ['item'],
		methods: {
			handleCheck(id) {
				// 通知App元件將對應的toDoItem物件的done屬性值取反
				// this.checkToDoItem(id);
				this.$bus.$emit('checkToDoItem', id);
			},
			handleDelete(id) {
				// 通知App元件將對應的toDoItem物件刪除
				if (window.confirm('確定刪除嗎?')) {
					// this.deleteToDoItem(id);
					// this.$bus.$emit('deleteToDoItem', id);
					pubsub.publish('deleteToDoItem', id);
				}
			},
			handleEdit() {
				if (this.item.hasOwnProperty('isEditing')) {
					this.item.isEditing = true; // 執行到此處並不會觸發重新解析模板
				} else {
					this.$set(this.item, 'isEditing', true);
				}
				this.$nextTick(() => {
					this.$refs.editInput.focus();
				});
			},
			/**
			 * 真正執行修改邏輯
			 */
			handleBlur({ id, title, isEditing, ...args }, evt) {
				if (evt.target.value.trim().length === 0) {
					return alert('輸入不可為空');
				} else {
					this.$bus.$emit('editToDoItem', {
						id,
						title: evt.target.value,
					});
				}
			},
		},
	};
</script>

<style scoped>
	/*item*/
	li {
		list-style: none;
		height: 36px;
		line-height: 36px;
		padding: 0 5px;
		border-bottom: 1px solid #ddd;
	}

	li label {
		float: left;
		cursor: pointer;
	}

	li label li input {
		vertical-align: middle;
		margin-right: 6px;
		position: relative;
		top: -1px;
	}

	li button {
		float: right;
		display: none;
		margin-top: 3px;
	}

	li:before {
		content: initial;
	}

	li:last-child {
		border-bottom: none;
	}

	li:hover {
		background-color: #ddd;
	}

	li:hover > button {
		display: inline-block;
	}

	.todo-enter-active {
		animation: atguigu 0.5s linear;
	}

	.todo-leave-active {
		animation: atguigu 0.5s linear reverse;
	}

	@keyframes atguigu {
		from {
			transform: translateX(100%);
		}
		to {
			transform: translateX(0px);
		}
	}
</style>
