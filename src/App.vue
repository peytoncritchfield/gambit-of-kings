<template>
  <div id="app">

    <div class="spaces-container">
      <div 
      class="spaces1" 
      v-for="(space, index) in boardSpaces" 
      v-bind:key="index"
      v-bind:class="{spaces2: !boardSpaces[index].color}"
      v-on:click="moveStart(index)">
      

      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  components: {},
  data() {
    return {
      boardSpaces: []
    }
  },
  methods: {
    createBoard() {
      for(let j = 0; j < 10; j++){
        for(let i = 0; i < 10; i++) {
          let spaces = {
            x: `${i + 1}`,
            y: `${j + 1}`,
            id: `(${i + 1},${j + 1})`
          }
          if ((i - j) % 2 === 0 || (j - i) % 2 === 0)
          {
            spaces.color = true
          } 
          

          this.boardSpaces.push(spaces)
        }
      }
    },
    moveStart(index) {
      index.innerHTML = 'hello'
    }
  },
  created() {
    this.createBoard()

  }
}
</script>

<style>
body {
  margin: 0;
  background-color: #2c3e50;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.spaces-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.spaces1 {
  background-color: #2c3e50;
  border: 1px solid white;
  display: flex;
  width: 9vw;
  height: 9vw;
  min-width: 50px;
  min-height: 50px;
  justify-content: center;
  align-items: center;
}
.spaces2 {
  background-color: black;
}
</style>
