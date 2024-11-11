<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<div class="flex min-h-screen items-center justify-center bg-purple-50 p-4">
	<div class="w-full max-w-md overflow-hidden rounded-xl bg-white shadow-lg">
		<div class="flex items-center bg-purple-500 p-4">
			<!-- <Menu className="text-white h-6 w-6" /> -->
			<h1 class="ml-4 text-xl font-medium text-white">Todo List</h1>
		</div>
		<div class="p-4">
			{@render children()}
		</div>
	</div>
</div>
