<script setup>
import axios from 'axios'
import PostIt from '../components/PostIt.vue'
import { ref, onMounted } from 'vue'

//var réactives pr ls données et ls champs du formulaire
const apiUrl = 'https://postit.zoul.dev/notes'
const notes = ref([])
const title = ref('')
const content = ref('')
//Methodes pr ls actions CRUD
const fetchData = async () => {
  //Recupt des donés
  const res = await axios.get(apiUrl)
  //stockage ds notes dans la var réactive
  notes.value = res.data.notes
}

const addNote = async () => {
  try {
    //pr send le titre e cntt à l'API
    await axios.post(apiUrl, {
      title: title.value,
      content: [content.value],
    })

    //pr vider le formulaire après l'ajout
    title.value = ''
    content.value = ''
    fetchData()
  } catch (error) {
    console.log("Erreur lors de l'ajout")
  }
}

const editNote = async (id) => {
  //recup ls news infos du user via des prompts
  const titre = prompt('Nouveau titre ?')
  const texte = prompt('Nouveau texte ?')

  //j'envoi les news infos à l'API pr update la note
  try {
    await axios.put(`${apiUrl}/${id}`, {
      title: titre,
      content: [texte],
    })

    //pr refresh ls notes à l'écran après la modif
    await fetchData()
  } catch (error) {
    console.log('Erreur')
  }
}

const removeNote = async (id) => {
  try {
    await axios.delete(`${apiUrl}/${id}`)
    await fetchData()
  } catch (error) {
    console.error('Erreur suppression :', error)
  }
}

//
onMounted(() => {
  fetchData()
})
</script>

<template>
  <div class="container">
    <header>
      <h1>Mes Post-it</h1>
    </header>

    <main class="grid-container">
      <form @submit.prevent="addNote">
        <input type="text" placeholder="Titre" v-model="title" />
        <textarea placeholder="Contenu" v-model="content"></textarea>
        <button type="submit">Ajouter</button>
      </form>

      <PostIt
        v-for="note in notes"
        :key="note._id"
        :id="note._id"
        :title="note.title"
        :content="note.content"
        :color="note.color"
        @delete-me="removeNote"
        @edit-me="editNote"
      />
    </main>
  </div>
</template>

<style>
.container {
  padding: 20px;
}
header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 30px;
}
/*design d'mn form*/
form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: #f4f4f4;
  padding: 15px;
  border-radius: 8px;
}
input,
textarea {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 4px;
}
</style>
