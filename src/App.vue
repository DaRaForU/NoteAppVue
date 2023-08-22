<script setup>
  import {ref} from 'vue';

  const showModal = ref(false);
  const newNote = ref('');
  const notes = ref(JSON.parse(localStorage.getItem('notes'))._value) || ref([]);

  const error = ref('');

  function RGBcolor() {
    var R = Math.floor(Math.random() * 256);
    var G = Math.floor(Math.random() * 256);
    var B = Math.floor(Math.random() * 256);
    var randomcolor = "rgb(" + R + "," + G + "," + B + ")";  
    return randomcolor;
  }

  function saveToLocal(){
    localStorage.setItem('notes', JSON.stringify(notes));
  }

  function deleteCard(id){
    notes.value.forEach((note, index) => {
      console.log(index)
      if(id === note.id){
        notes.value.splice(index, 1);
        saveToLocal();
      }
    });



    console.log(notes._value)
  }

  function removeCard(id){
    let updateCard = notes.value.filter((note) => note.id !== id);
    notes.value = updateCard;
  }


  const addNote = () => {
    if(newNote.value.length >= 10){
        notes.value.push({
        id: Math.floor(Math.random() * 100000), 
        text: newNote.value, 
        date: new Date().toLocaleDateString('en-US'),
        backgroundColor: RGBcolor()
      });

      newNote.value = '';
      showModal.value = false;
      error.value = '';

      saveToLocal();

    }else{
      error.value = 'Note needs to be 10 characters or more!';
    }

  };

  console.log(notes._value);



</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="error">{{ error }}</p>
        <button @click="addNote();">Add Note</button>
        <button @click="showModal = false; newNote = ''; error = '';" class="close">Close</button>
      </div>
    </div>


    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true;">+</button>
      </header>

      <div class="cards">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.backgroundColor}" :data-card-id="note.id">
          <button @click="deleteCard(note.id)">X</button>
          <p class="text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  .container{
    max-width: 1000px;
    margin: 0 auto;
  }
  header{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  header h1{
    font-size: 50px;
    font-weight: bold;
  }
  header button{
    padding: 10px;
    width: 50px;
    height: 50px;
    font-size: 25px;
    border-radius: 100%;
    cursor: pointer;
  }
  .cards{
    display: flex;
    flex-wrap: wrap;
  }

  .card{
    margin-right: 20px;
    margin-bottom: 20px;
    padding: 10px;
    width: 225px;
    height: 225px;
    background-color: aqua;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;

  }
  .card .text{
    color: white;
    overflow: hidden;
  }
  .card .date{
    color: white;
    font-weight: bold;
  }
  .card button{
    position: absolute;
    right: 2px;
    top: 2px;
    border-radius: 100%;
    padding: 10px;
    width: 10px;
    height: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: none;
    cursor: pointer;
  }

  .overlay{
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    z-index: 10;
  }
  .modal{
    width: 750px;
    background-color: white;
    padding: 30px;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
  }
  .modal button{
    padding: 10px;
    font-size: 20px;
    margin-top: 10px;
    border: none;
    border-radius: 5px;
    background-color: aqua;
    cursor: pointer;
  }
  .modal .close{
    background-color: rgb(235, 109, 109);
  }
  .modal p{
    color: red;
  }

</style>