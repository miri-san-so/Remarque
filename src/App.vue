<template>
  <div id="app">
    <Sidebar
      @newNote="updateNewNote"
      @oldNote="updateNote"
      ref="sidebar"
      class="hold-sidebar"
    />
    <Note @saved="updateSidebar" ref="note" />
  </div>
</template>

<script>
import Note from "./components/Note.vue";
import Sidebar from "./components/Sidebar.vue";

export default {
  name: "App",
  components: {
    Note,
    Sidebar,
  },
  methods: {
    updateNote(e) {
      this.$refs.note.setOldNote(e);
    },
    updateSidebar() {
      this.$refs.sidebar.forceRerender();
    },
    updateNewNote() {
      this.$refs.note.setNewNote();
    },
  },
  created() {
    window.addEventListener("paste", function(e) {
      // cancel paste
      e.preventDefault();

      // get text representation of clipboard
      var text = e.clipboardData.getData("text/plain");

      // insert text manually
      document.execCommand("insertHTML", false, text);
    });
  },
};
</script>

<style>
#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  overflow-x: hidden;
}
.hold-sidebar {
  transition: all 300ms ease;
  transform: translateX(-15vw);
  position: fixed;
  top: 0;
  left: 0;
}

.hold-sidebar:hover {
  transform: translateX(0);
}
</style>
