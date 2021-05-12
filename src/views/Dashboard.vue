<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-90 inset-0 grid place-items-center"
    >
      <DeletionConfirmation :id="selectedId" />
    </div>
    <div class="flex justify-center">
      <router-link
        :to="{ name: 'CreateNote' }"
        class="font-semibold border border-solid border-indigo-500 px-2 py-1"
        >Take a note</router-link
      >
    </div>
    <div
      class="grid grid-cols-5 space-x-2"
      v-for="note in notes"
      :key="note.id"
    >
      <router-link
        class="block border-b py-4 col-span-4"
        :to="{ name: 'Note', params: { id: note.id } }"
      >
        <div>
          <h3 class="text-xs text-indigo-500">
            {{ note.collection }}
          </h3>
          <h2 class="text-lg font-medium">{{ note.title }}</h2>
          <span class="text-gray-400 text-xs"> by {{ userId }}</span>
          <p class="overflow-ellipsis">{{ note.content }}</p>
          <p class="text-sm text-gray-400 mt-2 text-right">
            {{ new Date(note.createdAt).toLocaleDateString() }}
          </p>
        </div>
      </router-link>
      <div class="flex place-items-center">
        <button
          @click.prevent="deleteButton(note.id)"
          class="text-indigo-500 border border-gray-500 border-solid px-2 py-1 hover:bg-gray-300 hover:text-indigo-800"
        >
          Delete
        </button>
      </div>
    </div>
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
import DeletionConfirmation from "@/components/DeletionConfirmation.vue";

export default Vue.extend({
  name: "Dashboard",
  data() {
    return {
      notes: [],
      note: {},
      userId: this.$store.state.user.id,
      showConfirmation: false,
      selectedId: "",
    };
  },
  async mounted() {
    this.getNotes();
  },
  methods: {
    deleteButton(id: string) {
      this.showConfirmation = true;
      this.selectedId = id;
    },
    async getNotes() {
      const response = await axios.get("http://localhost:3000/notes");
      this.notes = response.data;
    },

    logout() {
      this.$store.commit("CLEAR");
      this.$router.push({ name: "Login" });
    },
  },
  components: {
    DeletionConfirmation,
  },
});
</script>
