<template>
  <div class="container">
    <div class="game">
      <div class="game-element top-left"></div>
      <div class="game-element top-right"></div>
      <div class="game-element bottom-left"></div>
      <div class="game-element bottom-right"></div>
    </div>
    <div class="controls">
      <form action="">
        <input type="radio" name="diff" value="easy" v-model="diff" />
        easy
        <input type="radio" name="diff" value="normal" v-model="diff" />
        normal
        <input type="radio" name="diff" value="hard" v-model="diff" />
        hard
      </form>
      <button>Start Game</button>
      <div>Round: {{ round }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      round: 1,
      diff: 'easy',
    };
  },
  methods: {
    getDiff(event) {
      this.diff = event;
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
  color: #2c3e50;
  margin-top: 60px;
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
.top-right {
  border-top-right-radius: 100%;
  background-color: red;
}
.bottom-left {
  border-bottom-left-radius: 100%;
  background-color: yellow;
}
.bottom-right {
  border-bottom-right-radius: 100%;
  background-color: blue;
}
</style>
