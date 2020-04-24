<template>
  <div class="menubar">
    <div class="newnote-btn" @click="newNote">
      <p >
        Add New Note
      </p>
      <button>
        <svg
          id="add-btn"
          width="14"
          height="14"
          viewBox="0 0 14 14"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M7 14L7 0M0 7L6 7M8 7L14 7"
            stroke="white"
            stroke-width="2"
          />
        </svg>
      </button>
    </div>
    <DropDown @noteTitle="addNote" :key="componentKey" />
  </div>
</template>

<script>
import DropDown from "./DropDown";
export default {
  name: "SideBar",
  components: {
    DropDown,
  },
  data() {
    return {
      componentKey: 0,
    };
  },
  methods: {
    addNote(e) {
      this.$emit("oldNote", e.innerText);
    },
    forceRerender() {
      this.componentKey += 1;
    },
    newNote() {
      this.$emit("newNote");
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Inter:400,600,700,800&display=swap");

@import url("https://fonts.googleapis.com/css?family=Manrope:400,500,600,700,800&display=swap");

* {
  font-family: "Manrope", sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: #f3f3f3;
}
.menubar {
  width: 18vw;
  height: 100vh;
  background-color: white;
  box-shadow: 0px 0px 50px rgba(0, 0, 0, 0.25);
}

.newnote-btn {
  cursor: pointer;
  width: inherit;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f6f6f6;
  height: 2.5em;
  border-bottom: 1px solid #1a1a1e;
  font-weight: 600;
}

.newnote-btn > button {
  cursor: pointer;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #1a1a1e;
  padding: 1em 1em;
}
.newnote-btn > button:focus {
  outline: none;
}

.newnote-btn:hover #add-btn {
  animation: up-down 300ms ease;
}
.newnote-btn > p {
  margin: 1em 1em;
}
</style>
