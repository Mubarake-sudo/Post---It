<template>
  <div class="postit-card" :style="{ backgroundColor: color }">
    <router-link :to="'/note/' + id" class="postit-link">
      <div class="postit-body">
        <h2>{{ title }}</h2>
        <p>{{ content?.[0] }}</p>
      </div>
    </router-link>

    <div class="postit-actions">
      <button @click.stop="$emit('edit-me', id)">Modifier la note</button>
      <button @click.stop="$emit('delete-me', id)" class="btn-del">Supprimer</button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: {
    id: String,
    title: String,
    content: Array,
    color: String,
  },
}
</script>

<style scoped>
.postit-card {
  padding: 15px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.6);
  border-radius: 40px;
}
.postit-body h2 {
  color: white;
  font-weight: bold;
  margin-bottom: 10px;
  word-wrap: break-word;
  white-space: normal;
}

.postit-body p {
  color: whitesmoke;
  margin: 5px 0;
  word-wrap: break-word;
  /*retr à la ligne pr ls txts depasant la largeur du bloc note */
  white-space: normal;
}
.postit-actions {
  display: flex;
  justify-content: space-between;
}
.btn-del {
  color: red;
  border: none;
  background: none;
  cursor: pointer;
}
</style>
