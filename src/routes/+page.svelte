<script lang="ts">
	type Todo = {
		id: string;
		text: string;
		done: boolean;
	};
	type Filter = 'all' | 'active' | 'completed';

	let todos = $state<Todo[]>([
		{ id: '1', text: 'Todo 1', done: false },
		{ id: '2', text: 'Todo 2', done: false }
	]);

	let filter = $state<Filter>('all');

	$effect(() => {
		const savedTodos = localStorage.getItem('todos');
		if (savedTodos) {
			todos = JSON.parse(savedTodos);
		}
	});

	$effect(() => {
		localStorage.setItem('todos', JSON.stringify(todos));
	});

	const filterTodos = (todos: Todo[]) => {
		if (filter === 'all') {
			return todos;
		}
		if (filter === 'active') {
			return todos.filter((todo) => !todo.done);
		}
		if (filter === 'completed') {
			return todos.filter((todo) => todo.done);
		}
		return [];
	};

	let filteredTodos = $derived(filterTodos(todos));

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

	const setFilter = (newFilter: Filter) => {
		filter = newFilter;
	};

	const remaining = () => {
		return todos.filter((todo) => !todo.done).length;
	};
</script>

<input onkeydown={addTodo} type="text" placeholder="add a todo" />

<div class="todos">
	{#each filteredTodos as todo, i}
		<div class="todo" class:completed={todo.done}>
			<input type="text" value={todo.text} oninput={editTodo} data-index={i} />
			<input type="checkbox" checked={todo.done} onchange={toggleTodo} data-index={i} />
		</div>
	{/each}
</div>

<div class="filters">
	<button onclick={() => setFilter('all')}>All</button>
	<button onclick={() => setFilter('active')}>Active</button>
	<button onclick={() => setFilter('completed')}>Completed</button>
</div>

<p>{remaining()} Remaining</p>

<style>
	.todos {
		display: grid;
		gap: 1rem;
		margin-block-start: 1rem;
	}

	.todo {
		position: relative;
		transition: opacity 0.3s;
	}

	.completed {
		opacity: 0.4;
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

	.filters {
		margin-block: 1rem;
	}
</style>
