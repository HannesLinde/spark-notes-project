<template>
  <div>
    <div
      class="w-screen h-screen fixed bg-red-700 opacity-90 inset-0 grid place-items-center z-10"
    >
      <div class="flex flex-col bg-white p-4">
        <h2 class="text-2xl p-2">Are you sure you want to delete this note?</h2>
        <div class="flex justify-center space-x-4">
          <button
            @click="deleteNote"
            class="bg-indigo-300 w-16 px-4 py-1 rounded-md border border-black hover:scale-105 transform transition-transform duration-200"
          >
            Yes
          </button>
          <button
            @click="$emit('complete')"
            class="bg-indigo-600 w-16 px-4 py-1 rounded-md border border-black hover:scale-105 transform transition-transform duration-200"
          >
            No
          </button>
        </div>
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
      notes: [],
    };
  },
  methods: {
    async deleteNote() {
      try {
        console.log("Hi");
        const response = await axios.delete(
          process.env.VUE_APP_API_SERVER_URL + "/notes/" + this.id,
          {
            headers: { authorization: this.$store.state.token },
          }
        );
        this.note = response.data;
        return response;
      } catch (error) {
        (this.submitError = "Sorry, your note could not be deleted due to: "),
          error;
      } finally {
        if (this.$route.params.id) {
          this.$router.push("/");
        }
        this.$emit("freshNotes");
        console.log("Message " + this.id + " was deleted!");
      }
    },
  },
  props: {
    id: {
      type: Number,
    },
  },
});
</script>

<style scoped>
</style>