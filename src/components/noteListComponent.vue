<script lang="ts" setup>
	import noteComponent from './noteComponent.vue';
	import { onMounted, ref } from 'vue';
	import type { Ref } from 'vue';

	interface note {
		content: string;
		backgroundColor: string;
	}

	const saveNoteToLocalStorage = (note: note) => {
		const notesOnLocalStorage = getNotesFromLocalStorage();
		notesOnLocalStorage.push(note);
		localStorage.setItem('notesList', JSON.stringify(notesOnLocalStorage));
	};

	const getNotesFromLocalStorage = (): note[] => {
		const notesJSON = localStorage.getItem('notesList');
		const validNotes = notesJSON ? JSON.parse(notesJSON) : [];
		notes.value = validNotes;
		return validNotes;
	};

	const getRandomColor = () => {
		return `background-color: hsl(${Math.random() * 360},100%, 75%);`;
	};

	const generateOneColor = getRandomColor();

	const notes: Ref<note[]> = ref([]);
	onMounted(() => {
		const validNotes = getNotesFromLocalStorage();
		notes.value = validNotes;
	});

	const createNoteContent = ref('');
	const createNoteInterface = ref(false);
	const noteOpened = ref(false);
	const floatNote = ref('');
	const floatNoteBackgroundColor = ref(getRandomColor());

	const clickNote = (noteContent: note) => {
		if (!noteOpened.value) {
			noteOpened.value = true;
			floatNote.value = noteContent.content;
			if (noteContent.backgroundColor)
				floatNoteBackgroundColor.value = noteContent.backgroundColor;
		}
	};

	const closeNote = () => {
		noteOpened.value = false;
		createNoteInterface.value = false;
	};

	const saveNote = () => {
		closeNote();
		floatNote.value = '';
		const noteContent = createNoteContent.value.trim();
		if (noteContent) {
			saveNoteToLocalStorage({
				content: noteContent,
				backgroundColor: getRandomColor(),
			});
			console.log({ noteContent });
		}
		createNoteContent.value = '';
	};

	const deleteNote = (noteContentToDelete: string) => {
		const notesOnLocalStorage = getNotesFromLocalStorage();
		console.log({ notesOnLocalStorage });
		const updatedNotes = notesOnLocalStorage.filter((note) => {
			return note.content !== noteContentToDelete;
		});
		localStorage.setItem('notesList', JSON.stringify(updatedNotes));
		notes.value = updatedNotes;
		closeNote();
	};
</script>

<template>
	<div id="notesListContainer">
		<div v-show="noteOpened" id="floatNote" :style="floatNoteBackgroundColor">
			{{ floatNote }}
			<span class="closeNoteButton" @click="closeNote"></span>
			<button
				type="button"
				id="deleteNoteButton"
				:style="generateOneColor"
				@click="() => deleteNote(floatNote)"
			>
				Delete Note
			</button>
		</div>
		<div
			v-show="createNoteInterface"
			id="floatNote"
			:style="floatNoteBackgroundColor"
		>
			<h2>Create a Note</h2>
			<span class="closeNoteButton" @click="closeNote"></span>
			<textarea
				name="noteContent"
				v-model="createNoteContent"
				:style="generateOneColor"
			></textarea>
			<button
				type="button"
				id="saveNoteButton"
				@click="saveNote"
				:style="generateOneColor"
			>
				Save Note
			</button>
			<span @click="closeNote"></span>
		</div>
		<noteComponent
			v-for="(note, index) in notes"
			:key="index"
			:content="note.content"
			:click-event="() => clickNote(note)"
			:style="note.backgroundColor"
		/>
		<span id="addNote" @click="() => (createNoteInterface = true)">+</span>
	</div>
</template>

<style scoped>
	h2 {
		text-align: center;
	}

	textarea {
		width: 100%;
		height: 340px;
		word-wrap: break-word;
		overflow: auto;
		font-size: 20px;
	}

	button {
		display: block;
		margin: 20px auto;
		border-radius: 20px;
		font-size: 32px;
		padding: 5px 10px;
		font-weight: bold;
	}

	#notesListContainer {
		display: flex;
		padding: 30px;
		width: 90%;
		flex-flow: row wrap;
	}

	#floatNote {
		position: fixed;
		color: black;
		top: 20%;
		left: calc(50% - 300px);
		width: 600px;
		height: 600px;
		font-size: 30px;
		padding: 20px 30px;
		word-wrap: break-word;
		z-index: 10;
		border-radius: 20px;
		overflow: auto;
		word-wrap: break-word;
	}

	#deleteNoteButton {
		margin: 100px auto;
	}

	.closeNoteButton {
		position: absolute;
		top: 1%;
		right: 1%;
		margin-left: 500px;
		width: 30px;
		height: 30px;
		clip-path: polygon(
			20% 0%,
			0% 20%,
			30% 50%,
			0% 80%,
			20% 100%,
			50% 70%,
			80% 100%,
			100% 80%,
			70% 50%,
			100% 20%,
			80% 0%,
			50% 30%
		);
		background-color: black;
		cursor: pointer;
		z-index: 15;
	}

	#saveNoteButton:hover {
		cursor: pointer;
	}

	#addNote {
		display: flex;
		flex-direction: column;
		font-weight: bold;
		font-size: 6em;
		margin: 0 20px;
		align-items: center;
		justify-content: center;
		transition: ease-out 0.01s;
	}

	#addNote:hover {
		transform: scale(1.2);
		transition: ease 0.1s all;
		cursor: pointer;
	}
</style>
