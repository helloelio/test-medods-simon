<template>
  <div class="container">
    <div class="game">
      <div ref="one" class="game-element top-left" @click="handlerGame(1)">
        <audio ref="clip1">
          <source src="https://s3.amazonaws.com/freecodecamp/simonSound1.mp3" />
        </audio>
      </div>

      <div ref="two" class="game-element top-right" @click="handlerGame(2)">
        <audio ref="clip2">
          <source src="https://s3.amazonaws.com/freecodecamp/simonSound2.mp3" />
        </audio>
      </div>

      <div ref="three" class="game-element bottom-left" @click="handlerGame(3)">
        <audio ref="clip3">
          <source src="https://s3.amazonaws.com/freecodecamp/simonSound2.mp3" />
        </audio>
      </div>

      <div ref="four" class="game-element bottom-right" @click="handlerGame(4)">
        <audio ref="clip4">
          <source src="https://s3.amazonaws.com/freecodecamp/simonSound2.mp3" />
        </audio>
      </div>
    </div>
    <div class="controls">
      <form>
        <input type="radio" name="diff" value="easy" v-model="diff" />Easy
        <input type="radio" name="diff" value="normal" v-model="diff" />
        Normal
        <input type="radio" name="diff" value="hard" v-model="diff" />
        Hard
      </form>
      <button class="btn" @click="startGame">Start Game</button>
      <div class="round-counter">Round: {{ round }}</div>
      <div class="winner" v-if="this.win">Congratulations your winner!</div>
      <div class="lost" v-if="this.lost">You lost</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      gameStarted: false,
      round: 1,
      diff: 'easy',
      order: [],
      playerOrder: [],
      flash: 0,
      intervalId: 0,
      turn: 1,
      good: true,
      compTurn: false,
      win: false,
      lost: false,
    };
  },

  computed: {
    diffInterval() {
      return this.diff === 'easy'
        ? 1500
        : this.diff === 'normal'
        ? 1000
        : this.diff === 'hard'
        ? 400
        : 0;
    },
  },

  methods: {
    getDiff(event) {
      this.diff = event;
      this.gameStarted = false;
      this.round = 1;
    },
    startGame() {
      this.gameStarted = true;
      this.round = 1;
      this.win = false;
      this.lost = false;
      this.order = [];
      this.playerOrder = [];
      this.flash = 0;
      this.intervalId = 0;
      this.turn = 1;
      this.good = true;
      for (let i = 0; i < 20; i += 1) {
        this.order.push(Math.floor(Math.random() * 4) + 1);
      }
      this.compTurn = true;
      this.intervalId = setInterval(this.gameTurn, this.diffInterval);
    },

    gameTurn() {
      if (this.flash == this.turn) {
        clearInterval(this.intervalId);
        this.compTurn = false;
      }

      if (this.compTurn) {
        setTimeout(() => {
          if (this.order[this.flash] == 1) this.lightingButton(1);
          if (this.order[this.flash] == 2) this.lightingButton(2);
          if (this.order[this.flash] == 3) this.lightingButton(3);
          if (this.order[this.flash] == 4) this.lightingButton(4);
          this.flash += 1;
        }, 200);
      }
    },

    lightingButton(index) {
      switch (index) {
        case 1:
          this.lightingButtonHelper(this.$refs.one, this.$refs.clip1);
          break;
        case 2:
          this.lightingButtonHelper(this.$refs.two, this.$refs.clip2);
          break;
        case 3:
          this.lightingButtonHelper(this.$refs.three, this.$refs.clip3);
          break;
        case 4:
          this.lightingButtonHelper(this.$refs.four, this.$refs.clip4);
          break;
      }
    },

    lightingButtonHelper(item, audio) {
      item.classList.add('light');
      audio.play();
      if (!this.win) {
        setTimeout(() => {
          item.classList.remove('light');
        }, 400);
      }
    },

    handlerGame(event) {
      if (this.gameStarted) {
        this.playerOrder.push(event);
        this.check();
      }
    },

    check() {
      if (
        this.playerOrder[this.playerOrder.length - 1] !==
        this.order[this.playerOrder.length - 1]
      ) {
        this.good = false;
        this.lost = true;
      }
      if (this.playerOrder.length == 5 && this.good) {
        this.winGame();
      }
      if (this.turn == this.playerOrder.length && this.good && !this.win) {
        this.turn += 1;
        this.round += 1;
        this.playerOrder = [];
        this.compTurn = true;
        this.flash = 0;
        this.intervalId = setInterval(this.gameTurn, this.diffInterval);
      }
    },
    winGame() {
      this.gameStarted = false;
      this.win = true;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  font-size: 1.5rem;
}
.btn {
  background: rgb(238, 174, 202);
  background: radial-gradient(
    circle,
    rgba(238, 174, 202, 1) 0%,
    rgba(148, 187, 233, 1) 100%
  );
  color: white;
  text-shadow: 1px 1px black;
  font-size: 1.5rem;
  padding: 10px 40px;
  margin: 10px 0;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  transition: 0.3s ease-in-out;
}
.btn:hover {
  transform: scale(1.01);
  box-shadow: 0px 0px 10px 0px black;
}
.game {
  display: grid;
  grid-template-columns: auto auto;
  max-width: 500px;
  margin: 0 auto 20px;
}
.controls {
  max-width: 500px;
  background-color: gray;
  color: white;
  margin: 0 auto;
}
.game-element {
  height: 200px;
  border: 10px solid #000;
  cursor: pointer;
}
.top-left {
  border-top-left-radius: 100%;
  background-color: green;
}
.top-left:active {
  opacity: 0.8;
}
.top-right {
  border-top-right-radius: 100%;
  background-color: red;
}
.top-right:active {
  opacity: 0.8;
}
.bottom-left {
  border-bottom-left-radius: 100%;
  background-color: yellow;
}
.bottom-left:active {
  opacity: 0.8;
}
.bottom-right {
  border-bottom-right-radius: 100%;
  background-color: blue;
}
.bottom-right:active {
  opacity: 0.8;
}
.light {
  opacity: 0.7;
}
.round-counter {
  padding: 10px;
  background-color: rgb(68, 19, 19);
}
.winner {
  padding: 10px;
  background-color: green;
}
.lost {
  padding: 10px;
  background-color: red;
}
</style>
