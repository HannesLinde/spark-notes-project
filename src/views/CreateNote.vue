<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <NavigationBar :showCreateButton="false" />

    <form @submit.prevent="submit" class="space-y-4">
      <div class="flex items-stretch">
        <label for="title" class="sr-only">Title</label>
        <input
          type="text"
          name="title"
          placeholder="Enter your note title"
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
      <select v-model="selectedCollection">
        <option value="Job">Job</option>
        <option value="ToDo">Todo</option>
        <option value="Personal" checked>Personal</option>
      </select>
      <div class="flex justify-center">
        <button
          type="submit"
          class="bg-indigo-500 py-1 px-2 text-white font-semibold rounded-lg hover:bg-indigo-300"
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
import NavigationBar from "@/components/NavigationBar.vue";

const now = new Date();

export default Vue.extend({
  data() {
    return {
      title: "",
      content: "",
      titleTouched: false,
      submitError: "",
      selectedCollection: "Personal",
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
          collection: this.selectedCollection,
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
  components: {
    NavigationBar,
  },
});
</script>

<style scoped>
</style>