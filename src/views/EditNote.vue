<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div class="flex justify-center">
      <h1 class="font-semibold underline px-2 py-1">Take a note</h1>
    </div>

    <form @submit.prevent="submit" class="space-y-4">
      <div>
        <label for="title" class="sr-only">Title</label>
        <input
          type="text"
          name="title"
          placeholder="Title"
          v-model="title"
          @blur="titleTouched = true"
          class="bg-transparent w-full text-center focus:outline-none border border-transparent focus:border-indigo-600"
        />
      </div>
      <p v-if="titleError" class="px-2 mt-1 text-xs text-red-600">
        {{ titleError }}
      </p>
      <div>
        <label for="content" class="sr-only">Content</label>
        <textarea
          placeholder="Content"
          v-model="content"
          rows="10"
          class="bg-transparent w-full focus:outline-none border border-transparent focus:border-indigo-600"
        />
      </div>
      <div class="flex justify-center">
        <button
          type="submit"
          class="bg-indigo-500 py-1 px-2 text-white font-semibold rounded-lg hover:bg-indigo-300"
        >
          Update
        </button>
      </div>
    </form>
    <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
      {{ submitError }}
    </p>
    <div class="grid grid-cols-3">
      <div></div>
      <div></div>
      <router-link
        :to="{ name: 'Dashboard' }"
        class="text-indigo-600 font-semibold hover:underline hover:text-indigo-300"
        >Back to Dashboard</router-link
      >
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";

const now = new Date();

export default Vue.extend({
  data() {
    return {
      title: "",
      content: "",
      titleTouched: false,
      submitError: "",
    };
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
          `http://localhost:3000/notes/${id}`,
          {
            title: this.title,
            content: this.content,
            updatedAt: now,
          }
        );
        this.$router.push("/");
        return response;
      } catch (error) {
        (this.submitError = "Sorry, your note could not be saved due to: "),
          error;
      }
    },
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
  async mounted() {
    const id = this.$route.params.id;
    const response = await axios.get("http://localhost:3000/notes/" + id);
    this.title = response.data.title;
    this.content = response.data.content;
  },
});
</script>

<style scoped>
</style>