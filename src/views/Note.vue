<template>
  <div class="py-6 px-4 max-w-screen-sm mx-auto">
    <header class="mb-4">
      <h2 class="text-indigo-500 text-lg">{{ note.collection }}</h2>
      <h1 class="text-4xl font-medium">{{ note.title }}</h1>
    </header>
    <p class="leading-relaxed text-lg">{{ note.content }}</p>
    <div class="text-sm text-right text-gray-400 mt-4">
      {{ new Date(note.createdAt).toLocaleDateString() }}
    </div>
    <router-link :to="{ name: 'Dashboard' }"
      >Back to notes dashboard</router-link
    >
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  data() {
    return {
      note: {},
    };
  },
  async mounted() {
    const id = this.$route.params.id;
    const response = await axios.get("http://localhost:3000/notes/" + id);
    this.note = response.data;
  },
});
</script>

<style scoped></style>
