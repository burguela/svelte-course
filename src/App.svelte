<svelte:options immutable={true} />

<script>
	import { tick } from 'svelte';
	import TodoList from './lib/TodoList.svelte';
	import { v4 as uuid } from 'uuid';

	let todoList;
	let showList = true;

	let todos = [
		{
			id: uuid(),
			title: 'Todo 1',
			completed: true
		},
		{
			id: uuid(),
			title: 'Todo 2',
			completed: false
		},
		{
			id: uuid(),
			title: 'Todo 3',
			completed: true
		},
		{
			id: uuid(),
			title: 'a long long long long long long long long long long long long long long long long long long long long long long long todo',
			completed: true
		}
	];

	async function handleAddTodo(event) {
		event.preventDefault();
		todos = [
			...todos,
			{
				id: uuid(),
				title: event.detail.title,
				completed: false
			}
		];

		await tick();
		todoList.clearInput();
	}

	function handleRemoveTodo(event) {
		todos = todos.filter((t) => t.id !== event.detail.id);
	}

	function handleToggleTodo(event) {
		todos = todos.map((todo) => {
			if (todo.id === event.detail.id) {
				return { ...todo, completed: event.detail.value };
			}
			return { ...todo };
		});
	}
</script>

<label>
	<input type="checkbox" bind:checked={showList} />
	Show/Hide List
</label>

{#if showList}
	<div style:max-width="400px">
		<TodoList
			{todos}
			bind:this={todoList}
			on:addTodo={handleAddTodo}
			on:removeTodo={handleRemoveTodo}
			on:toggleTodo={handleToggleTodo}
		/>
	</div>
{/if}

<style>
</style>
