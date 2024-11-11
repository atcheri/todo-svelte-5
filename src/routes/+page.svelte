<script lang="ts">
	let todos = $state([
		{ id: window.crypto.randomUUID(), text: 'Todo 1', done: false },
		{ id: window.crypto.randomUUID(), text: 'Todo 2', done: false }
	]);

	const addTodo = (event: KeyboardEvent) => {
		if (event.key !== 'Enter') {
			return;
		}

		const todoEl = event.target as HTMLInputElement;
		const id = window.crypto.randomUUID();
		const text = todoEl.value;

		todos = [...todos, { id, text, done: false }];
	};

	const editTodo = (e: Event) => {
		const inputEl = e.target as HTMLInputElement;
		const index = Number(inputEl.dataset.index);
		if (isNaN(index)) {
			return;
		}
		todos[index].text = inputEl.value;
	};

	const toggleTodo = (e: Event) => {
		const inputEl = e.target as HTMLInputElement;
		const index = Number(inputEl.dataset.index);
		if (isNaN(index)) {
			return;
		}
		todos[index].done = !todos[index].done;
	};
</script>

<input onkeydown={addTodo} type="text" placeholder="add a todo" />

<div class="todos">
	{#each todos as todo, i}
		<div class="todo">
			<input type="text" value={todo.text} oninput={editTodo} data-index={i} />
			<input type="checkbox" checked={todo.done} onchange={toggleTodo} data-index={i} />
		</div>
	{/each}
</div>

<style>
	.todos {
		display: grid;
		gap: 1rem;
		margin-block-start: 1rem;
	}

	.todo {
		position: relative;
	}

	input[type='text'] {
		width: 100%;
		padding: 1rem;
	}

	input[type='checkbox'] {
		position: absolute;
		right: 4%;
		top: 50%;
		translate: 0% -50%;
	}
</style>
