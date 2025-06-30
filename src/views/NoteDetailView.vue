<template>
  <div class="container">
    <router-link to="/" class="btn-back">RETOUR</router-link>

    <div v-if="note">
      <h1>{{ note.title }}</h1>
      <p>{{ note.content[0] }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'

const route = useRoute()
const note = ref('')

onMounted(async () => {
  const id = route.params.id
  const res = await axios.get(`https://postit.zoul.dev/notes/${id}`)
  note.value = res.data
})
</script>
