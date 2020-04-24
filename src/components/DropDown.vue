<template>
  <div class="dropdown">
    <button class="dropbtn" @click="myFunction()">
      Your Notes
      <svg
        @click="myFunction"
        style="transform: rotate(0deg);"
        class="icon-dropdown"
        width="12.4"
        height="7"
        viewBox="0 0 16 9"
        preserveAspectRatio="xMidYMid meet"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path d="M1 1L8 7.5L15 1" stroke="black" stroke-width="2" />
      </svg>
    </button>

    <div class="myDropdown" v-for="msg in prevNotes" v-bind:key="msg">
      <div class="note" :id="msg" :ref="msg">
        <a @click="showOldNote">{{ msg }}</a>
        <button @click="removeNote">
          <svg
            class="icon-remove"
            width="18"
            height="18"
            viewBox="0 0 18 18"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle cx="9" cy="9" r="8.5" stroke="black" />
            <line
              x1="4"
              y1="9"
              x2="14"
              y2="9"
              stroke="black"
              stroke-width="2"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DropDown",
  data() {
    return {
      prevNotes: [],
    };
  },
  methods: {
    showOldNote(e){
      this.$emit('noteTitle', e.target)
    },
    removeNote(e) {
      e.path.forEach((elem) => {
        if (elem.id != "app" && document.getElementById(elem.id) != null) {
          elem.parentElement.removeChild(elem);
          localStorage.removeItem(elem.getAttribute("id"));
        }
      });

    },
    myFunction() {
      let x = document.querySelectorAll(".note");
      for (let i = 0; i < x.length; i++) {
        if (x[i].style.display == "" || x[i].style.display == "flex") {
          x[i].style.display = "none";
        } else if (x[i].style.display == "none") {
          x[i].style.display = "flex";
        }
      }
      let drpdownicon = document.querySelector(".icon-dropdown");
      if (drpdownicon.style.transform == "rotate(0deg)") {
        drpdownicon.style.transform = "rotate(-90deg)";
      } else {
        drpdownicon.style.transform = "rotate(0deg)";
      }
    },
  },
  created() {
    localStorage.removeItem("loglevel:webpack-dev-server");
    this.prevNotes = Object.keys(localStorage);
  },
};
</script>

<style>
.dropdown {
  float: left;
  overflow: hidden;
  width: inherit;
}

.dropdown .dropbtn {
  user-select: none;
  cursor: pointer;
  font-size: 16px;
  border: none;
  outline: none;
  padding: 14px 16px;
  font-family: inherit;
  margin: 0;
  width: 100%;
  background-color: #f6f6f6;
}

.dropbtn {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10%;
  color: #afafaf;
}

.navbar a:hover,
.dropdown:hover .dropbtn,
.dropbtn:focus {
  background-color: #f6f6f6;
}

.dropdown-content {
  position: absolute;
  background-color: #f9f9f9;
  width: inherit;
  z-index: 1;
}

.note a {
  float: none;
  color: black;
  background-color: transparent;
  text-decoration: none;
  cursor: pointer;
}

.note a:hover {
  background-color: rgb(250, 250, 250);
}

.note {
  display: flex;
  justify-content: space-between;
  align-items: items;
  padding: 1em 0.5em;
}

.note button {
  cursor: pointer;
  border: none;
  height: 100%;
  background-color: transparent;
}

.show {
  display: block;
}

.icon-dropdown {
  pointer-events: none;
  transform: rotate(0deg);
  transition: all 200ms ease-in;
}
.note button:hover circle {
  fill: red;
  stroke: transparent;
}
.note button:hover line {
  stroke: white;
}
.note button:focus {
  outline: none;
}

@keyframes up-down {
  0% {
    transform: translateY(0px);
  }
  40% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
