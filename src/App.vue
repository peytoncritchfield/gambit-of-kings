<template>
  <div id="app">

    <div class="spaces-container">
      <div 
      class="spaces1" 
      v-for="(space, index) in boardSpaces" 
      v-bind:key="index"
      v-bind:class="{spaces2: !boardSpaces[index].color}"
      v-on:click="moveStart(index)">

        <img :src="imageSrc[index]" width="40" height="40"/>

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
      boardSpaces: [],
      imageSrc: [],
      darkPieceSetUp: ['death-knight', 'dire-wolf', 'dragon',
                       'ogre', 'lich', 'dark-castle', 'ogre',
                       'dragon', 'dire-wolf', 'death-knight',
                       'orc', 'skeleton', 'orc', 'skeleton',
                       'orc', 'skeleton', 'orc', 'skeleton',
                       'orc', 'skeleton'],
      lightPieceSetUp: ['soldier', 'elf', 'soldier',
                       'elf', 'soldier', 'elf', 'soldier',
                       'elf', 'soldier', 'elf',
                        'knight', 'lion', 'griffin',
                       'giant', 'light-castle', 'paladin', 'giant',
                       'griffin', 'lion', 'knight'],
      moveActivated: false
    }
  },
  methods: {
    createBoard() {
      for(let j = 0; j < 10; j++){
        for(let i = 0; i < 10; i++) {
          let spaces = {
            x: `${i + 1}`,
            y: `${j + 1}`,
            index: i + 10 * j
          }
          if ((i - j) % 2 === 0 || (j - i) % 2 === 0)
          {
            spaces.color = true
          } else {
            spaces.color = false
          }
          this.boardSpaces.push(spaces)
        }
      }
    },
    createPieces() {
      for(let i = 0; i < 100; i++) 
      {
        this.imageSrc.push(require('./assets/blank.png'))
      }
      for(let i = 0; i < 20; i++) {
        this.imageSrc.splice(i, 1, require(`./assets/dark/${this.darkPieceSetUp[i]}.png`))
      }
      for(let i = 0; i < 20; i++) {
        this.imageSrc.splice(i + 80, 1, require(`./assets/light/${this.lightPieceSetUp[i]}.png`))
      }
    },
    moveStart(index) {
      this.moveActivated = !this.moveActivated
      const spaceSelected = this.boardSpaces.find((child) => child.index === index)
      console.log(spaceSelected)
      if(this.moveActivated === true){
        console.log('ready to move')
      }
    }
  },

  created() {
    this.createBoard()
    this.createPieces()
    console.log(this.boardSpaces)
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: rgb(252, 252, 252);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  align-items: center;
  justify-content: center;
}
.spaces-container {
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  min-width: 500px;
  max-width: 650px
}
.spaces1 {
  background-color: #02779b;
  display: flex;
  width: 10vw;
  height: 10vw;
  min-width: 50px;
  min-height: 50px;
  max-width: 65px;
  max-height: 65px;
  justify-content: center;
  align-items: center;
}
.spaces2 {
  background-color: rgba(128, 127, 127, 0.5);
}
</style>
