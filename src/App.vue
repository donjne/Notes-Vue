<template>
  <!-- ... (rest of the template remains the same) ... -->
    <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <p @click="closeModal">x</p>
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10" />
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="saveNote">Save Note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          class="card"
          :style="{ backgroundColor: note.color, cursor: 'pointer' }"
          @click="showEditModal(note)"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";

const showModal = ref(false);
const selectedNote = ref(null);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

function getRandomColor() {
  const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

const addNote = () => {
  if (newNote.value.length < 5) {
    return (errorMessage.value = "Note should not be less than 5 characters");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    color: getRandomColor(),
    date: new Date(),
  });
  closeModal();
};

const showEditModal = (note) => {
  selectedNote.value = note;
  newNote.value = note.text;
  showModal.value = true;
};

const closeModal = () => {
  selectedNote.value = null;
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

const saveNote = () => {
  if (selectedNote.value) {
    selectedNote.value.text = newNote.value;
    closeModal();
  } else {
    if (newNote.value.length >= 5) {
      notes.value.push({
        id: Math.floor(Math.random() * 100000),
        text: newNote.value,
        color: getRandomColor(),
        date: new Date(),
      });
      closeModal();
    }
  }
};
</script>

<style scoped>
    .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  /* ... (rest of the style remains the same) */
  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
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

  .main-text {
    line-height: 1.25;
    font-size: 17.5px;
    font-weight: bold;
  }

  .date {
    font-size: 12.5px;
    margin-top: auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 1000px;
    color: white;
    font-size: 20px;
  }
  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 200vh;
    background-color: rgba(0, 0, 0, 0.77);
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10;
  }

  main {
    height: 100vh;
    width: 100vw;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;

  }

  .modal p {
    margin-left: auto;
    font-size: 20px;
    z-index: 100000;
    cursor: pointer;
  }

  textarea {
    width: 100%;
    height: 200px;
    padding: 5px;
    font-size: 20px;
  }

  .modal .error {
    color: red;
    margin: 0 auto;
  }
</style>
