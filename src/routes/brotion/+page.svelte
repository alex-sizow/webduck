<script>
	import { onMount } from 'svelte';

	/**
	 * @type {string[]}
	 * Массив элементов списка.
	 */
	let items = [];

	/**
	 * @type {string}
	 * Новый элемент, который будет добавлен в список.
	 */
	let newItem = '';

	/**
	 * Добавляет новый элемент в список, если он не пустой.
	 */
	function addItem() {
		if (newItem.trim() !== '') {
			items = [...items, newItem];
			newItem = '';
		}
	}

	/**
	 * Удаляет элемент из списка по индексу.
	 * @param {number} index - Индекс элемента, который нужно удалить.
	 */
	function removeItem(index) {
		items = items.filter((_, i) => i !== index);
	}
</script>

<div class="notion">
	<h1>Brotion</h1>
	<input type="text" bind:value={newItem} placeholder="Add a new item" />
	<button on:click={addItem}>Add</button>
	<ul>
		{#each items as item, index}
			<li>
				{item} <button on:click={() => removeItem(index)}>Remove</button>
			</li>
		{/each}
	</ul>
</div>

<style scoped>
	.notion {
		margin: 40px;
		display: flex;
		align-items: center;
		flex-direction: column;
	}
	h1 {
		font-size: 22px;
	}
	input {
		margin-bottom: 10px;
		padding: 5px;
		font-size: 16px;
	}
	button {
		margin-left: 5px;
		padding: 5px 10px;
		font-size: 16px;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		margin: 5px 0;
		display: flex;
		align-items: center;
	}
</style>
