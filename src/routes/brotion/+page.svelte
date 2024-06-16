<script>
	/** @type {string[]} */
	let items = [''];

	/** @type {string} */
	let newItem = '';

	/** @type {number | null} */
	let editingIndex = null;

	function addItem() {
		items = [...items, newItem];
		newItem = '';
	}

	function removeItem(index) {
		items = items.filter((_, i) => i !== index);
	}

	function finishEditing(index, event) {
		items[index] = event.target.textContent;
		editingIndex = null;
	}

	function isCursorAtEnd(event) {
		const target = event.target;
		if (!target) {
			console.error('Target is null');
			return false;
		}

		const selection = window.getSelection();
		if (!selection) {
			console.error('Selection is null');
			return false;
		}

		if (selection.rangeCount === 0) {
			console.error('No ranges in selection');
			return false;
		}

		const range = selection.getRangeAt(0);
		if (!range) {
			console.error('Range is null');
			return false;
		}

		const cursorPosition = range.endOffset;
		const textLength = target.textContent ? target.textContent.length : 0;

		return cursorPosition === textLength;
	}
</script>

<div class="notion">
	<h1>Brotion</h1>
	<input type="text" bind:value={newItem} placeholder="Add a new item" />

	<ul>
		{#each items as item, index}
			<li>
				<span>🔵</span>
				<div
					contenteditable="true"
					on:blur={(e) => finishEditing(index, e)}
					on:keydown={(e) => {
						if (e.key === 'Enter') {
							e.preventDefault(); // Предотвращаем перенос строки
							if (isCursorAtEnd(e)) {
								finishEditing(index, e);
								addItem();
							} else {
								console.log('dfsddfd');
							}
						}
					}}
				>
					{item}
				</div>
				<!-- <button on:click={() => removeItem(index)}>Remove</button> -->
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
		font-size: 42px;
		margin-bottom: 40px;
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
		display: block;
		margin: 5px 0;
		display: flex;
		align-items: center;
		min-width: 60px;
	}

	div {
		font-size: 24px;
	}
	div[contenteditable='true'] {
		cursor: text;
		padding: 5px;

		margin-top: 5px;
		min-width: 60px;
	}
	div[contenteditable='true']:focus {
		outline: none;
	}
</style>
