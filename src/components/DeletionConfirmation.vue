<template>
  <div>
    <!-- Modal window -->
    <div class="flex flex-col">
      <h2 class="text-2xl">Are you sure you want to delete this note?</h2>
      <div class="flex justify-center space-x-6">
        <button
          @click="deleteNote"
          class="bg-red-600 w-16 px-4 py-1 rounded-md border border-black hover:scale-105 transform transition-transform duration-200"
        >
          Yes
        </button>
        <button
          @click="showConfirmation = false"
          class="bg-green-600 w-16 px-4 py-1 rounded-md border border-black hover:scale-105 transform transition-transform duration-200"
        >
          No
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  data() {
    return {
      showConfirmation: false,
      note: {},
      submitError: "",
    };
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

<style scoped>
</style>