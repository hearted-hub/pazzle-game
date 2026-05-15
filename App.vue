<template>
  <div id="app">

    <!-- HEADER -->
    <div class="top-bar">

      <h1>15 PUZZLE GAME</h1>

      <!-- SETTINGS -->
      <div class="settings">

        <!-- MUSIC -->
        <audio
          ref="bgMusic"
          loop
        >
          <source
            src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3"
            type="audio/mpeg"
          >
        </audio>

        <button @click="toggleSound">
          🎵 Music :
          {{ sound ? "ON" : "OFF" }}
        </button>

        <button @click="changeTheme">
          🎨 Theme
        </button>

      </div>

    </div>

    <!-- LEVEL INFO -->
    <div class="level-bar">

      <h2>
        LEVEL {{ currentLevel }} / 20
      </h2>

      <h2>
        Moves : {{ moves }}
      </h2>

      <h2>
        Score : {{ finalScore }}/100
      </h2>

    </div>

    <!-- BOARD -->
    <div
      class="board"
      :class="theme"
    >

      <div
        v-for="(tile, index) in tiles"
        :key="index"
        class="tile"
        :class="{ empty: tile === '' }"
        @click="moveTile(index)"
      >

        {{ tile }}

      </div>

    </div>

    <!-- BUTTONS -->
    <div class="buttons">

      <button @click="shuffleTiles">
        Shuffle
      </button>

      <button @click="resetLevel">
        Reset
      </button>

      <button @click="nextLevel">
        Next Level
      </button>

    </div>

    <!-- LEVEL SELECTOR -->
    <div class="levels">

      <button
        v-for="level in 20"
        :key="level"
        @click="goToLevel(level)"
        :class="{
          active:
          currentLevel === level
        }"
      >

        {{ level }}

      </button>

    </div>

  </div>
</template>

<script>

export default {

  data() {

    return {

      currentLevel: 1,

      moves: 0,

      finalScore: 100,

      sound: true,

      theme: "wood",

      levels: [

        /* LEVEL 1 */
        [
          1,2,3,4,
          5,6,7,8,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 2 */
        [
          1,2,3,4,
          5,6,7,8,
          9,10,11,12,
          13,15,14,""
        ],

        /* LEVEL 3 */
        [
          1,10,2,13,
          4,"",8,11,
          6,3,15,12,
          9,14,7,5
        ],

        /* LEVEL 4 */
        [
          5,1,2,4,
          9,6,3,8,
          13,10,7,12,
          "",14,11,15
        ],

        /* LEVEL 5 */
        [
          2,3,4,8,
          1,6,7,12,
          5,10,11,15,
          9,13,14,""
        ],

        /* LEVEL 6 */
        [
          5,2,3,4,
          1,7,11,8,
          9,6,10,12,
          13,14,15,""
        ],

        /* LEVEL 7 */
        [
          8,1,2,4,
          5,6,3,7,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 8 */
        [
          1,6,2,4,
          5,10,3,8,
          9,14,7,12,
          13,11,15,""
        ],

        /* LEVEL 9 */
        [
          1,2,7,4,
          5,6,3,8,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 10 */
        [
          5,1,3,4,
          2,6,7,8,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 11 */
        [
          2,1,3,4,
          5,7,6,8,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 12 */
        [
          1,3,4,8,
          5,2,7,12,
          9,6,11,15,
          13,10,14,""
        ],

        /* LEVEL 13 */
        [
          4,1,2,8,
          5,6,3,12,
          9,10,7,15,
          13,14,11,""
        ],

        /* LEVEL 14 */
        [
          8,4,1,2,
          5,6,3,7,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 15 */
        [
          5,1,2,3,
          9,6,7,4,
          13,10,11,8,
          14,15,12,""
        ],

        /* LEVEL 16 */
        [
          2,6,3,4,
          1,10,7,8,
          5,9,11,12,
          13,14,15,""
        ],

        /* LEVEL 17 */
        [
          4,1,3,8,
          5,2,7,12,
          9,6,11,15,
          13,10,14,""
        ],

        /* LEVEL 18 */
        [
          8,1,4,2,
          5,6,3,7,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 19 */
        [
          2,1,7,4,
          5,6,3,8,
          9,10,11,12,
          13,14,15,""
        ],

        /* LEVEL 20 */
        [
          10,6,2,4,
          1,14,3,8,
          5,9,7,12,
          13,11,15,""
        ]

      ],

      tiles: []

    }

  },

  created() {

    this.loadLevel()

  },

  methods: {

    loadLevel() {

      this.tiles = [
        ...this.levels[
          this.currentLevel - 1
        ]
      ]

      this.moves = 0

      this.finalScore = 100

    },

    moveTile(index) {

      const emptyIndex =
        this.tiles.indexOf("")

      const validMoves = [

        emptyIndex - 1,
        emptyIndex + 1,
        emptyIndex - 4,
        emptyIndex + 4

      ]

      if (
        validMoves.includes(index)
      ) {

        [
          this.tiles[index],
          this.tiles[emptyIndex]
        ] = [

          this.tiles[emptyIndex],
          this.tiles[index]

        ]

        this.moves++

        this.calculateScore()

        this.checkWin()

      }

    },

    calculateScore() {

      let score =
        100 - (this.moves * 2)

      if (score < 0) {

        score = 0

      }

      this.finalScore = score

    },

    checkWin() {

      const win = [

        1,2,3,4,
        5,6,7,8,
        9,10,11,12,
        13,14,15,""

      ]

      if (

        JSON.stringify(this.tiles)
        ===
        JSON.stringify(win)

      ) {

        alert(
          "LEVEL "
          + this.currentLevel +
          " COMPLETED!"
        )

        if (
          this.currentLevel < 20
        ) {

          this.currentLevel++

          this.loadLevel()

        } else {

          alert(
            "YOU COMPLETED ALL LEVELS!"
          )

        }

      }

    },

    nextLevel() {

      if (
        this.currentLevel < 20
      ) {

        this.currentLevel++

        this.loadLevel()

      }

    },

    goToLevel(level) {

      this.currentLevel = level

      this.loadLevel()

    },

    resetLevel() {

      this.loadLevel()

    },

    shuffleTiles() {

      this.tiles =
        this.tiles.sort(
          () => Math.random() - 0.5
        )

    },

    toggleSound() {

      this.sound = !this.sound

      if (this.sound) {

        this.$refs.bgMusic.play()

      } else {

        this.$refs.bgMusic.pause()

      }

    },

    changeTheme() {

      if (
        this.theme === "wood"
      ) {

        this.theme = "dark"

      } else {

        this.theme = "wood"

      }

    }

  }

}
</script>

<style>

body{

  margin:0;

  font-family:Georgia;

  background:#a76b43;
}

#app{

  text-align:center;

  padding:20px;
}

/* TOP BAR */

.top-bar{

  display:flex;

  justify-content:
  space-between;

  align-items:center;

  color:white;
}

/* SETTINGS */

.settings button{

  margin-left:10px;
}

/* LEVEL BAR */

.level-bar{

  display:flex;

  justify-content:
  space-around;

  color:white;

  margin:20px 0;
}

/* BOARD */

.board{

  width:430px;

  margin:auto;

  display:grid;

  grid-template-columns:
  repeat(4,100px);

  gap:5px;

  padding:10px;

  border-radius:10px;

  border:8px solid #3b1f0f;
}

/* THEMES */

.wood{

  background:#5c3317;
}

.dark{

  background:#111;
}

/* TILE */

.tile{

  width:100px;

  height:100px;

  display:flex;

  justify-content:center;

  align-items:center;

  font-size:50px;

  font-weight:bold;

  cursor:pointer;

  border-radius:5px;

  transition:0.3s;
}

/* WOOD TILE */

.wood .tile{

  background:#e6c28b;

  color:#5a3210;

  border:2px solid #7a4b22;
}

/* DARK TILE */

.dark .tile{

  background:#333;

  color:white;

  border:2px solid #777;
}

/* EMPTY */

.empty{

  background:#5c2200 !important;

  border:none !important;

  cursor:default;
}

/* BUTTONS */

.buttons{

  margin-top:20px;
}

button{

  padding:12px 20px;

  margin:10px;

  border:none;

  border-radius:10px;

  background:#5c3317;

  color:white;

  cursor:pointer;

  font-size:18px;
}

button:hover{

  opacity:0.8;
}

/* LEVEL SELECTOR */

.levels{

  margin-top:30px;
}

.levels button{

  width:50px;

  height:50px;

  border-radius:50%;

  margin:5px;
}

/* ACTIVE LEVEL */

.active{

  background:blue !important;
}

h1,h2{

  color:white;

  text-shadow:
  2px 2px 5px black;
}

</style>