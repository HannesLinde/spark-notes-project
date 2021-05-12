<template>
  <div class="p-4 max-w-screen-sm mx-auto">
    <div
      v-if="showConfirmation"
      @click="showConfirmation = false"
      class="w-screen h-screen fixed bg-red-200 opacity-90 inset-0 grid place-items-center"
    >
      <DeletionConfirmation :id="selectedId" />
    </div>
    <NavigationBar :showDashboardButton="false" />
    <input
      v-if="$route.path == '/'"
      type="text"
      placeholder="search"
      v-model="keyword"
      @keyup="filteredNotes"
    />
    <!-- <NoteItem :notes="filteredNotes" /> -->

    <NoteItem v-for="note in notes" :key="note.id" :note="note" />
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import DeletionConfirmation from "@/components/DeletionConfirmation.vue";
import NavigationBar from "@/components/NavigationBar.vue";
import NoteItem from "@/components/NoteItem.vue";

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
  },
  components: {
    DeletionConfirmation,
    NavigationBar,
    NoteItem,
  },
});
</script>
