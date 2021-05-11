<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div class="flex flex-col">
      <h2 class="text-indigo-500 text-lg">{{ note.collection }}</h2>
    </div>

    <form @submit.prevent="submit" class="space-y-4">
      <div>
        <label for="title" class="sr-only">Title</label>
        <input
          type="text"
          name="title"
          placeholder="Title"
          v-model="title"
          @blur="
            titleTouched = true;
            submit;
          "
          class="bg-transparent w-full text-center text-2xl focus:outline-none border border-transparent focus:border-indigo-600"
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
          @blur="submit"
        />
      </div>
    </form>
    <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
      {{ submitError }}
    </p>
    <div class="grid grid-cols-3 grid-rows-2">
      <button @click="deleteNote">Delete</button>
      <div></div>
      <div></div>
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
      note: {},
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
        this.note = response.data;
        return response;
      } catch (error) {
        (this.submitError = "Sorry, your note could not be saved due to: "),
          error;
      }
    },
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
    this.note = response.data;
    this.title = response.data.title;
    this.content = response.data.content;
  },
});
</script>

<style scoped>
</style>