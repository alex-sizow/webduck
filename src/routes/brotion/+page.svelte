<script>
	import { tick } from 'svelte';

	let notes = [
		{ id: 1, text: 'go to hell' },
		{ id: 2, text: 'go to bread' },
		{ id: 3, text: 'i want milk' },
		{ id: 4, text: 'and other' },
		{ id: 5, text: 'something' }
	];

	const updateNoteText = (id, newText) => {
		const note = notes.find((note) => note.id === id);
		if (note) {
			note.text = newText;
		}
	};

	const newNote = async (index) => {
		const newId = notes.length + 1;
		const newNote = { id: newId, text: '' };
		notes = [...notes.slice(0, index + 1), newNote, ...notes.slice(index + 1)];
		await tick(); // Wait for DOM to update
		const newElement = document.getElementById(newId);
		if (newElement) {
			newElement.focus();
		}
	};
</script>

<div class="notion">
	<h1>Brotion</h1>

	<ul>
		{#each notes as { id, text }, index (id)}
			<li
				{id}
				contenteditable="true"
				on:input={(e) => {
					updateNoteText(id, e.target.innerText);
				}}
				on:keydown={async (e) => {
					if (e.key === 'Enter') {
						e.preventDefault();
						await newNote(index);
					}
				}}
			>
				{text}
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
