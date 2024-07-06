<script>
  import { tick } from 'svelte';

  let $state notes = [
    { id: 1, text: 'go to hell' },
    { id: 2, text: 'go to bread' },
    { id: 3, text: 'i want milk' },
    { id: 4, text: 'and other' },
    { id: 5, text: 'something' }
  ];

  let $state newElement = null;

  const updateNoteText = (id, newText) => {
    notes = notes.map(note => 
      note.id === id ? { ...note, text: newText } : note
    );
  };

  const newNote = async (index) => {
    const newId = notes.length + 1;
    const newNoteItem = { id: newId, text: '' };
    notes = [
      ...notes.slice(0, index + 1),
      newNoteItem,
      ...notes.slice(index + 1)
    ];

    await tick();
    if (newElement) {
      newElement.focus();
    }
  };

  $effect(() => {
    if (newElement) {
      newElement.focus();
    }
  });
</script>

<div class="notion">
	<h1>Brotion</h1>

	<ul>
		{#each notes as { id, text }, index (id)}
			<li
				bind:this={index === notes.length - 1 ? newElement : null}
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

<style>
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
		font-size: 24px;
		cursor: text;
		padding: 5px;
		margin-top: 5px;
	}
	li:focus {
		outline: none;
	}
</style>
