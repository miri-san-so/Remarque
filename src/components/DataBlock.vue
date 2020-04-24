<template>
  <div class="data-block" @mouseover="hoverIn()" @mouseleave="hoverOut()">
    <div class="data-type-option-list">
      <button class="open-option-list">+</button>
      <div class="data-type-option-list-values">
        <div class="option-text-size">
          <button class="heading-1-option" @click="makeHeading1()">
            H1
          </button>
          <button class="heading-2-option" @click="makeHeading2()">
            H2
          </button>
          <button class="heading-2-option" @click="makeHeading3()">
            H3
          </button>
          <button class="type-option" @click="makeHeading4()">
            H4
          </button>
        </div>

        <button
          class="add-option"
          @click="(event) => $emit('counterInc', event)"
        >
          Add Block
        </button>
        <button class="remove-option" @click="removeDataBlock()">
          Remove
        </button>
      </div>
    </div>

    <button class="drag-btn" draggable="false">
      <svg
        width="3"
        height="16"
        viewBox="0 0 3 16"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <circle cx="1.5" cy="2.16666" r="1.5" fill="#676767" />
        <circle cx="1.5" cy="8.16666" r="1.5" fill="#676767" />
        <circle cx="1.5" cy="14.1667" r="1.5" fill="#676767" />
      </svg>
    </button>

    <div class="data-block-text">
      <div
        inputmode="text"
        class="data-block-text-value"
        placeholder="sad"
        contenteditable="true"
        data-text="type your notes here"
        :locked="lck"
      ></div>
    </div>
  </div>
</template>


<script src="https://cdn.jsdelivr.net/npm/sortablejs@latest/Sortable.min.js"></script>
<script>
import draggable from "vuedraggable";
export default {
  name: "DataBlock",
  data() {
    return {
      allText: [],
      lck: "false",
    };
  },
  components: {
    draggable,
  },
  methods: {
    removeDataBlock() {
      if(this.$el.children[2].children[0].getAttribute('locked') == "false"){
        this.$el.parentElement.removeChild(this.$el);
      }
    },
    hoverIn() {
      this.$el.children[0].style.opacity = 1;
      this.$el.children[1].style.opacity = 1;
    },
    hoverOut() {
      this.$el.children[0].style.opacity = 0;
      this.$el.children[1].style.opacity = 0;
    },
    addListeners() {
      this.allText.push(document.querySelectorAll(".data-block")[0]);

      this.allText.forEach((element) => {
        element.addEventListener("mouseover", () => {
          element.children[1].style.opacity = 1;
          element.children[0].style.opacity = 1;
        });
        element.addEventListener("mouseleave", () => {
          element.children[1].style.opacity = 0;
          element.children[0].style.opacity = 0;
        });
      });
    },
    makeHeading1() {
      let block_text = this.$el.children[2].children[0];
      if (block_text.getAttribute("locked") == "false") {
          block_text.style.fontSize = "2.5rem";
          block_text.style.fontWeight = "800";
      }
    },
    makeHeading2() {
      let block_text = this.$el.children[2].children[0];

      if (block_text.getAttribute("locked") == "false") {

        block_text.style.fontSize = "1.8rem";
        block_text.style.fontWeight = "600";
      }
    },
    makeHeading3() {
      let block_text = this.$el.children[2].children[0];

      if (block_text.getAttribute("locked") == "false") {

        block_text.style.fontSize = "1.5rem";
        block_text.style.fontWeight = "400";
      }
    },
    makeHeading4() {
      let block_text = this.$el.children[2].children[0];

      if (block_text.getAttribute("locked") == "false") {

        block_text.style.fontSize = "1.2rem";
        block_text.style.fontWeight = "400";
      }
    },
  },
  mounted() {
    window.addEventListener("paste", function(e) {
      // cancel paste
      e.preventDefault();
      // get text representation of clipboard
      var text = e.clipboardData.getData("text/plain");
      // insert text manually
      document.execCommand("insertHTML", false, text);
    });
    // let el = document.querySelector(".remarque-note");
    let el = document.querySelector("#app > div.remarque-note");
    // let allText = [];
    let queue = [];
    let cgkQueue = [];
    
    this.addListeners();
  },
};
</script>

<style scoped>
.data-block {
  display: flex;
}

.data-block > .data-type-option-list {
  height: 0;
  background: rgb(197, 197, 197);
  opacity: 0;
  display: flex;
  align-items: flex-end;
  flex-direction: column;
  transition: all 300ms ease-in-out;
}

.data-block > .data-type-option-list > .open-option-list {
  font-size: 1rem;
  cursor: pointer;
  border: none;
  color: #ffffff;
  padding: 0.8em 1em;
  background-color: rgb(46, 46, 46);
  /* padding: 0.7em 0.5em; */
}

.data-block > .data-type-option-list > .data-type-option-list-values {
  display: none;
}
.data-block > .data-type-option-list > .data-type-option-list-values > button {
  padding: 0.8em 2.5em;
  background-color: rgb(46, 46, 46);
  color: white;
  border: none;
  font-size: 0.8rem;
}

.data-block
  > .data-type-option-list
  > .data-type-option-list-values
  > .option-text-size {

  display: flex;
}

.data-block
  > .data-type-option-list
  > .data-type-option-list-values
  > .option-text-size
  > button {
  flex-grow: 1;
      background-color: rgb(46, 46, 46);
  color: white;  border: none;
  padding: 0.4rem;
}
.data-block
  > .data-type-option-list
  > .data-type-option-list-values
  > .option-text-size
  > button:hover {
  background-color: #252525;
}
.data-block
  > .data-type-option-list
  > .data-type-option-list-values
  > button:hover {
  background-color: #252525;
}
.data-block
  > .data-type-option-list
  > .data-type-option-list-values
  > button:focus {
  outline: none;
}

.data-block > .data-type-option-list:hover .data-type-option-list-values {
  display: flex;
  flex-direction: column;
  position: absolute;
  height: 20vh;
}

.drag-btn {
  opacity: 0;
  font-size: 1.4em;
  text-align: center;
  font-weight: 400;
  width: max-content;
  height: max-content;
  border: none;
  color: black;
  padding: 0.3em 0.6em;
  background: #fff;
  cursor: grab;
}

.drag-btn:focus {
  outline: none;
}

.data-block-text > .data-block-text-value {
  min-width: 60vw !important;
  max-width: 60vw !important;
  height: auto;
  word-wrap: break-word;
  word-break: keep-all;
  background-color: #fff;
  padding: 10px 5px;
  padding-left: 0;
  font-size: 1.2rem;
}

[contentEditable="true"]:empty:not(:focus):before {
  content: attr(data-text);
  font-weight: 800;
  opacity: 0.5;
}

.data-block > .data-block-text:hover .data-type-option-list {
  opacity: 0;
  display: none;
  background-color: #fff;
}

.data-block-text > .data-block-text-value:focus {
  outline: none;
}

input[type="checkbox"] {
  margin: 1rem;
  width: 1rem;
  height: 1rem;
}
</style>
