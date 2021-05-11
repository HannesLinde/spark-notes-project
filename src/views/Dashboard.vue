<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-80 inset-0 grid place-items-center"
    >
      <DeletionConfirmation />
    </div>
    <div class="flex justify-center">
      <router-link
        :to="{ name: 'CreateNote' }"
        class="font-semibold border border-solid border-indigo-500 px-2 py-1"
        >Take a note</router-link
      >
    </div>
    <router-link
      class="block border-b py-4"
      :to="{ name: 'Note', params: { id: note.id } }"
      v-for="note in notes"
      :key="note.id"
    >
      <div>
        <header class="mb-2">
          <h3 class="text-xs text-indigo-500">
            {{ note.collection }}
          </h3>
          <h2 class="text-lg font-medium">{{ note.title }}</h2>
          <span class="text-gray-400 text-xs"> by {{ userId }}</span>
        </header>
        <p>{{ note.content }}</p>
        <p class="text-sm text-gray-400 mt-2 text-right">
          {{ new Date(note.createdAt).toLocaleDateString() }}
        </p>
      </div>
      <button
        @click="showConfirmation = true"
        class="text-indigo-500 border border-gray-500 border-solid px-2 py-1 hover:bg-gray-300 hover:text-indigo-800"
      >
        Delete
      </button>
      <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
        {{ submitError }}
      </p>
    </router-link>
    <div class="flex mt-8 justify-end">
      <button @click="logout" class="text-sm hover:text-indigo-600">
        Logout
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  name: "Dashboard",
  data() {
    return {
      notes: [],
      note: {},
      userId: this.$store.state.user.id,
      submitError: "",
      showConfirmation: false,
    };
  },
  async mounted() {
    this.getNotes();
  },
  methods: {
    async getNotes() {
      const response = await axios.get("http://localhost:3000/notes");
      this.notes = response.data;
    },

    logout() {
      this.$store.commit("CLEAR");
      this.$router.push({ name: "Login" });
    },
    async deleteNote(id: string | number) {
      try {
        console.log(id);
        const response = await axios.delete(
          `http://localhost:3000/notes/${id}`
        );
        this.note = response.data;
        return response;
      } catch (error) {
        this.submitError =
          "Sorry, your note could not be deleted due to: " + error;
      } finally {
        this.getNotes();
        this.$router.push("/");
        console.log("Message " + id + " was deleted!");
      }
    },
  },
});
</script>
