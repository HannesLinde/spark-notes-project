<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div class="flex justify-center">
      <h1 class="font-semibold underline px-2 py-1">Take a note</h1>
    </div>

    <form
      @submit.prevent="submit"
      class="flex flex-col place-items-center space-y-4"
    >
      <div class="flex items-stretch">
        <label for="title" class="sr-only">Title</label>
        <input
          type="text"
          name="title"
          placeholder="Title"
          v-model="title"
          @blur="titleTouched = true"
          class="click:border click:border-solid click:border-indigo-500"
        />
      </div>
      <p v-if="titleError" class="px-2 mt-1 text-xs text-red-600">
        {{ titleError }}
      </p>
      <div>
        <label for="content" class="sr-only">Content</label>
        <textarea placeholder="Content" v-model="content" />
      </div>
      <div>
        <button
          type="submit"
          class="bg-indigo-500 py-1 px-2 text-white font-semibold rounded-lg hover:bg-indigo-200"
        >
          Create
        </button>
      </div>
    </form>
    <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
      {{ submitError }}
    </p>
    <router-link :to="{ name: 'Dashboard' }" class="margin hover:underline"
      >Back to Dashboard</router-link
    >
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
        const response = await axios.post("http://localhost:3000/notes", {
          title: this.title,
          content: this.content,
          collection: "Personal",

          createdAt: now,
          updatedAt: now,
          createdBy: this.$store.state.user.id,
        });
        console.log(response);
        this.$router.push("/");
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
});
</script>

<style scoped>
</style>