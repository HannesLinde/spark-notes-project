<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-90 inset-0 grid place-items-center"
    >
      <DeletionConfirmation :id="selectedId" />
    </div>
    <NavigationBar
      :showDashboardButton="false"
      @update:keyword="keyword = $event"
    />
    <div v-if="filteredNotes.length != 0">
      <h2 class="text-2xl">Search</h2>
      <NoteList :notes="filteredNotes" />
    </div>
    <div class="flex flex-start space-x-4">
      <h2
        class="text-xs tracking-wide cursor-pointer text-indigo-800 hover:underline"
        @click="collection = ''"
      >
        #AllNotes
      </h2>
      <span
        class="text-xs tracking-wide text-indigo-500 hover:underline cursor-pointer"
        @click="collection = 'ToDo'"
        >#ToDos</span
      >
      <span
        class="text-xs tracking-wide text-indigo-500 hover:underline cursor-pointer"
        @click="collection = 'Job'"
        >#Job</span
      >
      <span
        class="text-xs tracking-wide text-indigo-500 hover:underline cursor-pointer"
        @click="collection = 'Personal'"
        >#Personal</span
      >
    </div>
    <div v-if="filteredNotes.length == 0">
      <NoteList
        :notes="getCollection"
        @request:delete="showConfirmation = true"
      />
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import DeletionConfirmation from "@/components/DeletionConfirmation.vue";
import NavigationBar from "@/components/NavigationBar.vue";
import NoteList from "@/components/NoteList.vue";

export default Vue.extend({
  name: "Dashboard",
  data() {
    return {
      notes: [],
      note: {},
      userId: this.$store.state.user.id,
      showConfirmation: false,
      selectedId: "",
      keyword: "",
      collection: "",
    };
  },
  async mounted() {
    this.getNotes();
  },
  methods: {
    deleteButton(id) {
      this.showConfirmation = true;
      this.selectedId = id;
    },
    async getNotes() {
      const response = await axios.get("http://localhost:3000/notes");
      this.notes = response.data;
    },
  },
  computed: {
    filteredNotes() {
      if (this.keyword.length < 2) {
        return [];
      }
      return this.notes.filter((note) => {
        return (
          note.title.toLowerCase().includes(this.keyword.toLowerCase()) ||
          note.content.toLowerCase().includes(this.keyword.toLowerCase())
        );
      });
    },
    getCollection() {
      if (this.collection == "") {
        return this.notes;
      }
      return this.notes.filter((note) => note.collection == this.collection);
    },
  },
  components: {
    DeletionConfirmation,
    NavigationBar,
    NoteList,
  },
});
</script>
