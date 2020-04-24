<template>
  <div class="remarque-note">
    <div class="note-options">
      <button @click="addNewLine()" class="new-line-btn">Add Line</button>
      <button @click="lockNote()" class="lock-btn">
        Unlocked
        <svg
          width="12"
          height="18"
          viewBox="0 0 14 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12 8V4C11.8333 3 11 1 9 1C8.2 1 7.33333 1 7 1"
            stroke="#FEFEFE"
          />
          <rect x="0.5" y="8" width="13" height="11" stroke="#FEFEFE" />
          <circle cx="7" cy="13.5" r="2" fill="#FEFEFE" />
        </svg>
      </button>
      <button @click="saveNote()" class="save-btn">Save</button>
    </div>
    <div
      class="note-title"
      data-text="Untitled"
      contenteditable="true"
      inputmode="text"
      :locked="lck"
    ></div>
    <div v-for="index in count" :key="index" class="wrapper">
      <DataBlock v-on:counterInc="CounterInc" />
    </div>
  </div>
</template>

<script>
import DataBlock from "./DataBlock.vue";
export default {
  name: "Note",
  data() {
    return {
      count: 1,
      lck: "false",
    };
  },
  components: {
    DataBlock,
  },
  methods: {
    CounterInc() {
      if (this.lck == "false") {
        this.count += 1;
      }
    },

    lockNote() {
      if (this.lck == "true") {
        this.lck = "false";
      } else {
        this.lck = "true";
      }
      let a = document.querySelector(".note-title");
      let x = document.querySelectorAll(".data-block");
      for (let i = 0; i < x.length; i++) {
        let y = x[i].children[2].children[0];
        let test = y.getAttribute("locked");
        if (test == "false") {
          this.$el.children[0].children[1].innerText = "Locked";
          this.$el.children[0].children[1].innerHTML += `<svg style="margin-left: 0.6rem;" width="12" height="18" viewBox="0 0 14 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M2 8V4C2.16667 3 3 1 5 1C5.8 1 6.66667 1 7 1" stroke="#FEFEFE"/>
            <path d="M12 8V4C11.8333 3 11 1 9 1C8.2 1 7.33333 1 7 1" stroke="#FEFEFE"/>
            <rect x="0.5" y="8" width="13" height="11" stroke="#FEFEFE"/>
            <circle cx="7" cy="13.5" r="2" fill="#FEFEFE"/>
            </svg>
            `;
          a.setAttribute("contenteditable", false);
          y.setAttribute("contenteditable", "false");
          y.setAttribute("locked", "true");
        } else {
          this.$el.children[0].children[1].innerText = "Unlocked";
          this.$el.children[0].children[1].innerHTML += `<svg style="margin-left: 0.6rem;" width="12" height="18" viewBox="0 0 14 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M12 8V4C11.8333 3 11 1 9 1C8.2 1 7.33333 1 7 1" stroke="#FEFEFE"/>
            <rect x="0.5" y="8" width="13" height="11" stroke="#FEFEFE"/>
            <circle cx="7" cy="13.5" r="2" fill="#FEFEFE"/>
            </svg>
            `;
          a.setAttribute("contenteditable", true);
          y.setAttribute("contenteditable", "true");
          y.setAttribute("locked", "false");
        }
      }
    },

    saveNote() {
      this.$emit("saved");
      let a = document.querySelector(".note-title").innerText;
      let x = document.querySelectorAll(".data-block-text-value");
      let oldNote = localStorage.getItem(a);
      if (a.replace(/ {0,}/, "") != "") {
        if (oldNote != null) {
          let temp = {};
          for (let i = 0; i < x.length; i++) {
            let temp2 = {
              value: x[i].innerText,
              weight: x[i].style.fontWeight == "" ? 400 : x[i].style.fontWeight,
              size: x[i].style.fontSize == "" ? "1.2rem" : x[i].style.fontSize,
            };
            temp[i] = temp2;
          }
          if (a != "loglevel:webpack-dev-server") {
            localStorage.setItem(a, JSON.stringify(temp));
          }
        } else {
          let temp = {};
          for (let i = 0; i < x.length; i++) {
            let temp2 = {
              value: x[i].innerText,
              weight: x[i].style.fontWeight == "" ? 400 : x[i].style.fontWeight,
              size: x[i].style.fontSize == "" ? "1.2rem" : x[i].style.fontSize,
            };
            temp[i] = temp2;
          }
          localStorage.setItem(a, JSON.stringify(temp));
        }
      }
    },

    addNewLine() {
      if (this.lck == "false") {
        this.count += 1;
      }
    },

    setOldNote(e) {
      this.count = 0;
      let x = JSON.parse(localStorage.getItem(e));
      document.querySelector(".note-title").innerText = e;
      let lines = Object.entries(x);
      for (let i = 0; i < lines.length; i++) {
        this.CounterInc();
      }
      this.addText(lines);
    },
    addText(arr) {
      console.log(arr);
      // let x = [];
      setTimeout(() => {
        let l = document.querySelector(".remarque-note");
        for (let i = 2; i < 2 + arr.length; i++) {
          let data_block_text_value =
            l.children[i].children[0].children[2].children[0];
          data_block_text_value.innerText = arr[i - 2][1].value;
          data_block_text_value.style.fontWeight = arr[i - 2][1].weight;
          data_block_text_value.style.fontSize = arr[i - 2][1].size;

          // data_block_text_value.setAttribute("style",`font-weight: ${arr[i-2][1].weight}`);
          // data_block_text_value.setAttribute("style",`font-size: ${arr[i-2][1].size}`);
        }
        // console.log(l);
      }, 100);
    },

    setNewNote() {
      console.log("hello?");
      this.saveNote();
      document.querySelector(".note-title").innerText = "";
      this.count = 1;
      document.querySelector(".data-block-text-value").innerText = "";
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Inter:400,600,700,800&display=swap");
@import url("https://fonts.googleapis.com/css?family=Manrope:400,500,600,700,800&display=swap");
* {
  margin: 0;
  padding: 0;
  font-family: "Manrope", sans-serif !important;
}

body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.remarque-note {
  border-radius: 10px;
  margin-top: 2%;
  margin-bottom: 5rem;
  display: flex;
  flex-direction: column;
  background-color: #fff;
  padding: 1rem 1rem;
  align-items: center;
}

.note-options {
  display: inline-flex;
  width: 100%;
  justify-content: flex-end;
}

.note-options > button {
  display: flex;
  justify-content: space-evenly;
  border-radius: 0.5rem;
  align-self: flex-end;
  padding: 0.8rem 2.5rem;
  border: none;
  background-color: #1d1d20;
  color: white;
  font-weight: bold;
  letter-spacing: 1px;
  cursor: pointer;
  margin: 1rem 0.25rem;
}

.note-options > button > svg {
  margin-left: 0.6rem;
}

.note-options > button:hover {
  background-color: #494950;
}
.note-options > button:focus {
  outline: none;
}

.note-title {
  min-width: 60vw !important;
  max-width: 60vw !important;
  margin-left: 3.8rem !important;
  font-size: 4.5rem;
  font-weight: 800;
  padding-bottom: 5px;
  border-bottom: 1px solid black;
  margin-bottom: 2rem;
}

.note-title:focus {
  outline: none;
}
[contentEditable="true"]:empty:not(:focus):before {
  content: attr(data-text);
  font-weight: 800;
  opacity: 0.5;
}
</style>
