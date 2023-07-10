<script setup>
import { ref } from 'vue';
const isModalOpen = ref(false);
const isEditModal = ref(false);
const text = ref("");
const editId = ref(null);
const errorMessage = ref("");
const notes = ref([]);

function randomColor() {
  return 'hsl(' + (Math.random() * 360) + ', 100%, 75%)';
}
const addNote = () => {

  if (text.value.length < 5) {
    return errorMessage.value = "Enter atleast 5 characters"
  }

  notes.value.push({
    id: Math.floor(Math.random() * 10000),
    text: text.value,
    date: new Date(),
    background: randomColor()
  });
  text.value = "";
  isModalOpen.value = false;
  errorMessage.value = "";
}

const editNote = (id) => {
  isEditModal.value = true;
  const updatedNoteIndex = notes.value.findIndex(note => note.id === id);
  text.value = notes.value[updatedNoteIndex].text;
  editId.value = id;
}

const updateNotes = () => {
  isEditModal.value = true;
  const updatedNoteIndex = notes.value.findIndex(note => note.id === editId.value);
  notes.value[updatedNoteIndex].text = text.value;
  editId.value = null;
  text.value = "";
  isEditModal.value = false;
}


const deleteNote = id => {
  const updatedNote = notes.value.filter(note => note.id !== id);
  notes.value = updatedNote;
}

</script>

<template>
  <main>
    <div v-if="isModalOpen" class="overlay">
      <div class="modal">
        <textarea v-model.trim="text" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="isModalOpen = false, text = ''">Close</button>
      </div>
    </div>
    <div v-if="isEditModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="text" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="() => updateNotes()">Save</button>
        <button class="close" @click="isEditModal = false, text = ''">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="isModalOpen = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" :style="{ backgroundColor: note.background }" class="card">
          <p class="main-text">{{ note.text }}</p>
          <div class="bottom">
            <p class="date">{{ note.date.toLocaleDateString("en-IN") }}</p>
            <div class= "action">
              <button class="btn-primary" @click="()=>editNote(note.id)">Edit</button>
              <button class="btn-secondary" @click="()=>deleteNote(note.id)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 90vw
}

.container {
  max-width: 800px;
  padding: 10px;
  margin: 0 auto
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
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  /* background-color: rgb(160 190 232); */
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
  width: 98vw;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
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
  background-color: #1499d9;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px
}

.modal .close {
  background-color:rgb(128 121 132);;
  margin-top: 7px;
}

.error {
  color: brown;
}

.bottom{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.btn-primary{
  background: #1499d9;
  color: #ffffff;
  cursor: pointer;
  padding: 5px 7px;
  border: none;
  border-radius: 20%;
}
.btn-secondary{
  background: rgb(128 121 132);
  color: #ffffff;
  cursor: pointer;
  padding: 5px 7px;
  border: none;
  border-radius: 20%;
}


</style>