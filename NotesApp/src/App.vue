<script setup>
  import {ref} from "vue"

  const showModal = ref(false)
  const newNote = ref("")
  const errorMessage = ref("")
  const notes = ref([])

  const getRandomColor = () => {
    return "hsl("+ Math.random() * 360  + ", 100%, 85%)"
  }

  const addNote = () => {
    if (newNote.value.trim().length < 5) {
      errorMessage.value = "Note needs to be 5 characters or more"
      return
    }

    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value,
      date: new Date(),
      bgColor: getRandomColor()
    })

    newNote.value = ""
    showModal.value = false
  }
</script>

<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote()">Add Note</button>
        <button class="close" @click="showModal=false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal=true">+</button>
      </header>

      <div class="cards-container">
        <div v-for="note in notes" 
          class="card"
          :key="note.id"
          :style="{backgroundColor:note.bgColor}">
          
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("id-ID") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main{
    height:100vh;
    width:100vw;
  }

  .container {
    max-width: 1280px;
    padding: 1rem;
    margin: 0 auto;
  }

  header {
    display:flex;
    justify-content: space-between;
    align-items: center;
  }

  h1{
    font-weight: bold;
    margin-bottom: 1.5rem;
    font-size:2.5rem;
    color:black
  }

  header button {
    border: none;
    padding: 1rem;
    height:50px;
    width:50px;
    cursor:pointer;
    background-color: black;
    border-radius: 100%;
    color: white;
    font-size: 1em;
  }

  .date {
    font-size:0.95em;
    font-weight:bold;

  }

  .cards-container{
    display: flex;
    flex-wrap: wrap;
  }

  .card {
    width:225px;
    height:225px;
    background-color: rgb(224, 176, 43);
    padding: 1rem;
    border-radius: 15px;
    display:flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
    color: black;
  }

  .overlay {
    position:absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display:flex;
    align-items:center;
    justify-content:center;
  }
  
  .modal{
    width: 768px;
    background-color: white;
    border-radius: 10px;
    padding:30px;
    position: relative;
    display:flex;
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

  .modal .close {
    background-color: rgb(192, 15, 15);
    margin-top: 7px;
  }

  .modal p {
    color:red
  }
</style>
