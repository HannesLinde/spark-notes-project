<template>
  <div class="py-6 px-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-80 inset-0 grid place-items-center"
    >
      <DeletionConfirmation :id="this.$route.params.id" />
    </div>
    <NavigationBar />
    <header class="flex flex-col mb-4">
      <h2 class="text-indigo-500 text-lg">{{ note.collection }}</h2>
      <input
        v-model="title"
        @blur="submit"
        class="text-4xl font-medium bg-gray-100 p-2 focus:outline-none border border-transparent focus:border-indigo-600"
      />
      <p v-if="titleError" class="px-2 mt-1 text-xs text-red-600">
        {{ titleError }}
      </p>
      <h3 class="text-sm text-gray-400 mt-4">
        <a :href="`mailto:${$store.state.user.email}`" class="hover:underline"
          >Mail note creator</a
        >
      </h3>
    </header>
    <input
      v-model="content"
      placeholder="Content"
      rows="10"
      class="bg-transparent w-full bg-gray-100 p-2 focus:outline-none border border-transparent focus:border-indigo-600"
      @blur="submit"
    />
    <div class="text-sm text-right text-gray-400 my-4">
      {{ new Date(note.createdAt).toLocaleDateString() }}
    </div>
    <div class="flex justify-center space-x-4">
      <button
        @click="showConfirmation = true"
        class="text-red-600 border border-gray-500 boder border-solid rounded-md px-2 py-1 hover:bg-red-600 hover:text-indigo-800"
      >
        Delete
      </button>
    </div>
    <p v-if="submitError" class="px-2 mt-1 text-xs text-red-600">
      {{ submitError }}
    </p>
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
    const response = await axios.get("http://localhost:3000/notes/" + id);
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
  },
  components: {
    DeletionConfirmation,
    NavigationBar,
  },
});
</script>

<style scoped></style>
