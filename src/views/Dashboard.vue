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
    <div v-if="filteredNotes.length == 0">
      <h2 class="text-2xl">All my notes</h2>
      <NoteList :notes="notes" @request:delete="showConfirmation = true" />
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
    todoItems() {
      return this.notes.filter((note) => note.collection == "ToDo");
    },
    personalItems() {
      return this.notes.filter((note) => note.collection == "Personal");
    },
    jobItems() {
      return this.notes.filter((note) => note.collection == "Job");
    },
    randomItems() {
      return this.notes.filter((note) => note.collection == "");
    },
  },
  components: {
    DeletionConfirmation,
    NavigationBar,
    NoteList,
  },
});
</script>
