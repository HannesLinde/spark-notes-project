<template>
  <div>
    <div class="py-6 px-4 max-w-screen-sm mx-auto">
      <header class="flex flex-col mb-4">
        <h2 class="text-indigo-500 text-lg">{{ note.collection }}</h2>
        <h1 class="text-4xl font-medium">{{ note.title }}</h1>
        <h3 class="text-sm text-gray-400 mt-4">
          <a :href="`mailto:${$store.state.user.email}`" class="hover:underline"
            >Mail note creator</a
          >
        </h3>
      </header>
      <p class="leading-relaxed text-lg">{{ note.content }}</p>
      <div class="text-sm text-right text-gray-400 mt-4">
        {{ new Date(note.createdAt).toLocaleDateString() }}
      </div>
      <div class="grid grid-cols-3 space-x-4">
        <router-link
          :to="{ name: 'Dashboard' }"
          class="text-gray-500 border border-indigo-500 border-solid px-2 py-1 hover:bg-indigo-300 hover:text-gray-800"
          >Back to notes dashboard</router-link
        >
        <button
          @click="deleteNote"
          class="text-indigo-500 border border-gray-500 border-solid px-2 py-1 hover:bg-gray-300 hover:text-indigo-800"
        >
          Delete
        </button>
        <router-link
          :to="`/edit-note/${$route.params.id}`"
          class="text-gray-500 border border-indigo-500 border-solid px-2 py-1 hover:bg-indigo-300 hover:text-gray-800"
          >Edit note</router-link
        >
      </div>
      <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
        {{ submitError }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  data() {
    return {
      note: {},
      submitError: "",
    };
  },
  async mounted() {
    const id = this.$route.params.id;
    const response = await axios.get("http://localhost:3000/notes/" + id);
    this.note = response.data;
  },
  methods: {
    async deleteNote() {
      const id = this.$route.params.id;
      try {
        console.log("Hi");
        const response = await axios.delete(
          `http://localhost:3000/notes/${id}`
        );
        this.note = response.data;
        return response;
      } catch (error) {
        (this.submitError = "Sorry, your note could not be deleted due to: "),
          error;
      } finally {
        this.$router.push("/");
        console.log("Message " + id + " was deleted!");
      }
    },
  },
});
</script>

<style scoped></style>
