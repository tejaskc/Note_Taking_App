<script setup lang="ts">
import { ref } from "vue";
import Note from "../components/Note.vue";

const showModal = ref(false);
const noteId = ref(0);
const newNote = ref("");
const errorMessage = ref("");

interface Notes {
  id: number;
  text: string;
  date: Date;
  backgroundColor: string;
}
const notes = ref<Notes[]>([]);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

const editNote = (id: number) => {
  console.log("Edit id", id);
  noteId.value = id;
  showModal.value = true;
  let note = notes.value.find((n) => n.id == id);
  if (note) newNote.value = note.text;
};

const updateNote = () => {
  if (noteId.value) {
    let index = notes.value.findIndex((n) => n.id == noteId.value);
    if (index > -1) {
      notes.value[index].text = newNote.value;
      showModal.value = false;
    }
  }
};

const deleteNote = (id: number) => {
  let index = notes.value.findIndex((n) => n.id == id);
  if (index > -1) {
    notes.value.splice(index, 1);
  }
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote" v-if="noteId == 0">Add Note</button>
        <button @click="updateNote" v-else>Update Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h2>Notes</h2>
        <button
          @click="
            showModal = true;
            newNote = '';
            noteId = 0;
          "
        >
          +
        </button>
      </header>
      <div class="cards-container mt-5">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <Note :note="note" @editNote="editNote" @deleteNote="deleteNote"/>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 5px;
  width: 40px;
  height: 40px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 650px;
  height: 80%;
  background-color: white;
  border-radius: 10px;
  padding: 50px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 17px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.modal p {
  color: rgb(193, 15, 15);
}
</style>
