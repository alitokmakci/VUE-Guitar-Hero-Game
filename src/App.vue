<template>
  <div class="counter">
    <div class="counts">
      <h1>Correct Notes: {{ correctCount }}</h1>
      <h1>Wrong Notes: {{ wrongCount }}</h1>
      <h1>Missed Notes: {{ missedCount }}</h1>
    </div>
  </div>
  <div class="video-container">
    <video src="/nem.mp4" id="video"></video>
  </div>
  <div class="scene">
    <button v-if="!gameStarted" class="start-btn" @click="gameStarted = true">
      START
    </button>
    <div class="fret">
      <div class="notes">
        <div
          class="note"
          v-for="(note, index) in notes"
          :key="index"
          :id="`note_${index}`"
          :class="note.color"
          :data-note="note.color"
          :data-top="10"
        ></div>
      </div>
      <div class="line g">
        <div class="switch green"></div>
      </div>
      <div class="line y">
        <div class="switch yellow"></div>
      </div>
      <div class="line r">
        <div class="switch red"></div>
      </div>
      <div class="line b">
        <div class="switch blue"></div>
      </div>
      <div class="line o">
        <div class="switch orange"></div>
      </div>
      <div class="line c">
        <div class="switch cyan"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";

const fretHeight = 600;
const BPM = 142;
const counter = 8;
const beat = 60000 / BPM; // 422ms
const delay = 300;
const switchPosition = fretHeight - 35;
const gameStarted = ref(false);
const correctCount = ref(0);
const wrongCount = ref(0);
const missedCount = ref(0);

const notes = [
  {
    color: "green",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "green",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "green",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "green",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "cyan",
    value: 32,
  },
  {
    color: "orange",
    value: 16,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "green",
    value: 8,
  },
  {
    color: "blue",
    value: 8,
  },
  {
    color: "orange",
    value: 8,
  },
  {
    color: "cyan",
    value: 8,
  },
  {
    color: "orange",
    value: 16,
  },
  {
    color: "cyan",
    value: 16,
  },
  {
    color: "blue",
    value: 8,
  },
];

const play = () => {
  let timeout = delay;
  let length = counter;
  for (let i = 0; i < notes.length; i++) {
    if (i !== 0) {
      length = notes[i - 1].value;
      timeout = timeout + beat / (length / counter);
    }

    setTimeout(() => {
      const noteId = String(`note_${i}`);

      const noteEl = document.getElementById(noteId);

      let top = 10;

      const interval = setInterval(() => {
        noteEl.style.display = "block";
        noteEl.style.top = `${top}px`;
        noteEl.dataset.top = top;
        top += fretHeight / beat;

        if (top > fretHeight) {
          clearInterval(interval);
          noteEl.dataset.missed = "1";
        }
      }, 1);
    }, timeout);
  }
};

setInterval(() => {
  const missedNotes = document.querySelectorAll('[data-missed="1"]');
  missedCount.value = missedNotes.length;
}, 100);

watch(gameStarted, () => {
  const video = document.getElementById("video");
  play();
  setTimeout(() => {
    video.play();
  }, 750);
});

const checkNoteIsCorrect = (color) => {
  /* Check query selector is enough? */
  const firstNote = document.querySelector(`.note.${color}`);

  if (firstNote === null) {
    return false;
  }

  const topPositionOfTheNote = firstNote.dataset.top;

  if (
    topPositionOfTheNote < switchPosition + 50 &&
    topPositionOfTheNote > switchPosition - 50
  ) {
    firstNote.remove();
    return true;
  }

  return false;
};

onMounted(() => {
  const green_switch = document.querySelector(".switch.green");
  const yellow_switch = document.querySelector(".switch.yellow");
  const red_switch = document.querySelector(".switch.red");
  const blue_switch = document.querySelector(".switch.blue");
  const orange_switch = document.querySelector(".switch.orange");
  const cyan_switch = document.querySelector(".switch.cyan");

  document.addEventListener("keypress", (e) => {
    let result = false;

    if (e.repeat) {
      return;
    }

    if (e.key === "q") {
      green_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("green");
    }
    if (e.key === "w") {
      yellow_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("yellow");
    }
    if (e.key === "e") {
      red_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("red");
    }
    if (e.key === "r") {
      blue_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("blue");
    }
    if (e.key === "t") {
      orange_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("orange");
    }
    if (e.key === "y") {
      cyan_switch.style.transform = "translateY(2px)";
      result = checkNoteIsCorrect("cyan");
    }

    if (result) {
      correctCount.value = correctCount.value + 1;
    } else {
      wrongCount.value = wrongCount.value + 1;
    }
  });

  document.addEventListener("keyup", (e) => {
    if (e.key === "q") {
      green_switch.style.transform = "translateY(0)";
    }
    if (e.key === "w") {
      yellow_switch.style.transform = "translateY(0)";
    }
    if (e.key === "e") {
      red_switch.style.transform = "translateY(0)";
    }
    if (e.key === "r") {
      blue_switch.style.transform = "translateY(0)";
    }
    if (e.key === "t") {
      orange_switch.style.transform = "translateY(0)";
    }
    if (e.key === "y") {
      cyan_switch.style.transform = "translateY(0)";
    }
  });
});
</script>

<style lang="scss">
@font-face {
  font-family: "Nightmare Hero";
  src: url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.eot");
  src: url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.eot?#iefix")
      format("embedded-opentype"),
    url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.woff2")
      format("woff2"),
    url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.woff")
      format("woff"),
    url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.ttf")
      format("truetype"),
    url("//db.onlinewebfonts.com/t/46b53f0fc8c8ceace63065da38c05771.svg#Nightmare Hero")
      format("svg");
}

* {
  font-family: "Nightmare Hero" !important;
}
body {
  margin: 0;
}

#app {
  width: 100vw;
  height: 100vh;
  background: rgba(50, 50, 50, 0.15);
}
.scene {
  width: 100%;
  height: 100%;
  perspective: 400px;
  position: fixed;
  bottom: 300px;
}

.fret {
  width: 400px;
  height: 600px;
  //height: 1000px;
  background: #ba883d;
  position: absolute;
  bottom: 0;
  left: calc(50% - 200px);
  transform-style: preserve-3d;
  transform: rotateX(45deg);
  //box-shadow: 0px -4px 3px #000;
  display: flex;
  align-items: center;
  justify-content: space-evenly;

  &::before {
    content: "";
    position: absolute;
    top: 10px;
    height: 3px;
    width: 100%;
    z-index: 0;
    //background-color: rgba(255, 255, 255, 0.2);
    box-shadow: 0px -15px 10px #fff;
  }
}

.line {
  height: 100%;
  background-color: #dbcb9e;
  width: 6px;
  position: relative;
}

.switch {
  position: absolute;
  bottom: 35px;
  left: -16px;
  height: 16px;
  width: 40px;
  border-radius: 50%;

  &:before {
    content: "";
    width: 20px;
    height: 8px;
    background-color: white;
    border-radius: 50%;
    position: absolute;
    top: 3.5px;
    left: 9px;
  }

  &:after {
    content: "";
    position: absolute;
    display: block;
    border-radius: 50%;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: white;
    z-index: 1;
    background: radial-gradient(
      ellipse at top,
      rgba(0, 0, 0, 0) 40%,
      rgba(0, 0, 0, 0.2) 60%,
      rgba(0, 0, 0, 0.6) 95%,
      rgba(0, 0, 0, 1) 100%
    );
  }
}

.green {
  background-color: green;
}

.blue {
  background-color: blue;
}

.red {
  background-color: red;
}

.orange {
  background-color: orange;
}

.yellow {
  background-color: yellow;
}

.cyan {
  background-color: cyan;
}

.notes {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
}

.note {
  position: absolute;
  top: 10px;
  height: 16px;
  width: 40px;
  border-radius: 50%;
  z-index: 10;
  display: none;

  &.green {
    left: 35px;
  }

  &.yellow {
    left: 75px;
  }

  &.red {
    left: 115px;
  }

  &.blue {
    left: 210px;
  }

  &.orange {
    left: 268px;
  }

  &.cyan {
    left: 325px;
  }
}

.video-container {
  width: 100vw;
  height: 100vh;
}

video {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100vw;
  height: 100vh;
  transform: translate(-50%, -50%);
  object-fit: cover;
}

.start-btn {
  z-index: 15;
  position: fixed;
  top: 50%;
  left: 45.5%;
  font-size: 50px;
  padding: 10px 20px;
  border: 0;
  background-color: white;
  border-radius: 12px;
}

.counter {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
  color: white;
  font-size: 20px;
  letter-spacing: 1px;
  margin-left: 20px;

  h1 {
    font-family: Arial, Helvetica, sans-serif !important;
  }
}
</style>
