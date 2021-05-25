<template>
  <div class="py-6 px-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-80 inset-0 grid place-items-center"
    >
      <DeletionConfirmation :id="this.$route.params.id" />
    </div>
    <NavigationBar :showSearchBar="false" />
    <header class="flex flex-col bg-gray-200 rounded-tl-lg rounded-tr-lg">
      <div class="flex justify-between">
        <h2 class="text-indigo-500 text-lg mx-2">#{{ note.collection }}</h2>
        <div class="text-sm p-2">
          {{ new Date(note.createdAt).toLocaleDateString() }}
        </div>
      </div>
      <input
        type="text"
        v-model="title"
        @blur="submit"
        class="text-4xl tracking-tight shadow-md font-medium py-2 mx-2 mb-4 rounded-md bg-gray-100 focus:outline-none border border-transparent focus:border-indigo-600"
      />
      <p v-if="titleError" class="px-2 mt-1 text-xs text-red-600">
        {{ titleError }}
      </p>
    </header>
    <main class="bg-gray-400 py-4 px-2 rounded-bl-lg rounded-br-lg">
      <textarea
        v-model="content"
        placeholder="Content"
        rows="10"
        class="text-lg w-full bg-gray-100 rounded-md shadow-md focus:outline-none border border-transparent focus:border-indigo-600"
        @blur="submit"
      />

      <div class="flex justify-center space-x-4">
        <button
          @click="showConfirmation = true"
          class="text-sm tracking-wide border border-gray-500 border-solid rounded-md px-2 py-1 bg-indigo-300 shadow-sm hover:scale-105 transform transition-transform duration-200"
        >
          Delete
        </button>
      </div>
      <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
        {{ submitError }}
      </p>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";
import DeletionConfirmation from "../components/DeletionConfirmation.vue";
import NavigationBar from "@/components/NavigationBar.vue";

const now = new Date();

export default Vue.extend({
  data() {
    return {
      note: {},
      content: "",
      title: "",
      submitError: "",
      titleTouched: false,
      showConfirmation: false,
    };
  },
  async mounted() {
    const id = this.$route.params.id;
    const response = await axios.get(
      process.env.VUE_APP_API_SERVER_URL + "/notes/" + id,
      {
        headers: { authorization: this.$store.state.token },
      }
    );
    this.note = response.data;
    this.content = response.data.content;
    this.title = response.data.title;
  },
  computed: {
    titleError() {
      if (!this.titleTouched || this.title.length >= 1) {
        return "";
      } else {
        return "Please enter a title";
      }
    },
  },
  methods: {
    validate() {
      this.titleTouched = true;
      if (this.titleError) return false;
      return true;
    },
    async submit() {
      console.log("Submitttti!");
      if (!this.validate()) return;

      try {
        const id = this.$route.params.id;
        const response = await axios.patch(
          process.env.VUE_APP_API_SERVER_URL + id,
          {
            title: this.title,
            content: this.content,
            updatedAt: now,
          },
          {
            headers: { authorization: this.$store.state.token },
          }
        );
        this.note = response.data;
        console.log(this.note);
        console.log("Updated!");
        return response;
      } catch (error) {
        (this.submitError = "Sorry, your note could not be saved due to: "),
          error;
      }
    },
  },
  components: {
    DeletionConfirmation,
    NavigationBar,
  },
});
</script>

<style scoped></style>
