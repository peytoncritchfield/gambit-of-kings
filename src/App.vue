<template>
  <div id="app">

    <div class="spaces-container">
      <div 
      class="spaces1" 
      v-for="(space, index) in boardSpaces" 
      v-bind:key="index"
      v-bind:class="{spaces2: !boardSpaces[index].color}"
      v-on:click="move(index)">

        {{boardSpaces[index].index}}

        <img :src="imageSrc[index]" width="35" height="40"/>

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
      moveFromMoveTo: [],
      lightsMove: true,
      availableMoves: [],
      directions: [
        {moveX: 1, moveY: -1}, {moveX: 1, moveY: 0}, {moveX: 1, moveY: 1}, 
        {moveX: 0, moveY: 1}, {moveX: -1, moveY: 1}, {moveX: -1, moveY: 0}, 
        {moveX: -1, moveY: -1}, {moveX: 0, moveY: -1}]
    }
  },
  methods: {
    createBoard() {
      for(let j = 0; j < 10; j++){
        for(let i = 0; i < 10; i++) {

          let spaces = {
            x: i,
            y: j,
            index: i + 10 * j
          }

          if (j === 0) {
            spaces.piece = this.darkPieceSetUp[i]
          } else if (j === 1) {
            spaces.piece = this.darkPieceSetUp[i + 10]
          } else if (j > 1 && j < 8) {
            spaces.piece = 'empty'
            spaces.team = 'none'
            
          } else if (j === 8) {
            spaces.piece = this.lightPieceSetUp[i]
          } else if (j === 9) {
            spaces.piece = this.lightPieceSetUp[i + 10]
          }


          if (j < 2) {
            spaces.team = 'dark'
            spaces.turn = false
          } else if (j > 7) {
            spaces.team = 'light'
            spaces.turn = true

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
    move(index) {
      let movePhase = this.moveFromMoveTo.length
      this.moveFromMoveTo.push(index)
      let original = this.moveFromMoveTo[0]
      let next = this.moveFromMoveTo[1]

      


      if (movePhase === 0) {
        this.prepareAvailableMoves(index)
      }
      

      if(
        movePhase === 1 
        && this.boardSpaces[original].piece !== 'light-castle'
        && this.boardSpaces[original].piece !== 'dark-castle')
      {
        if (this.boardSpaces[original].piece !== 'empty') {
          if (this.boardSpaces[original].turn === true) {

            for(let i = 0; i < this.availableMoves.length; i++) {
              if (this.boardSpaces[next].index === this.availableMoves[i]) {
                
                this.imageSrc.splice(original, 1, require('./assets/blank.png'))
                this.imageSrc.splice(next, 1, require(`./assets/${this.boardSpaces[original].team}/${this.boardSpaces[original].piece}.png`))
               
    
                this.boardSpaces[next].piece = this.boardSpaces[original].piece
                this.boardSpaces[next].team = this.boardSpaces[original].team
                this.boardSpaces[original].team = 'none'
                this.boardSpaces[next].turn = this.boardSpaces[original].turn
                this.boardSpaces[original].turn = 'null'
                this.boardSpaces[original].piece = 'empty'
    
                for(let i = 0; i < 100; i++) {
                  if(this.boardSpaces[i].team === 'dark') {
                    this.boardSpaces[i].turn = !this.boardSpaces[i].turn
                  } else if (this.boardSpaces[i].team === 'light') {
                    this.boardSpaces[i].turn = !this.boardSpaces[i].turn
                  }
                } 
              }
            }
          }
        }
        this.availableMoves = []
        this.moveFromMoveTo = []
        if(this.boardSpaces[5].turn === true) {
          console.log('Dark\'s Turn')
        } else {
          console.log('Light\'s Turn')
        }
      } else if (movePhase === 1) {
        alert("Illegal Move")
        this.moveFromMoveTo = []

      }
    },
    prepareAvailableMoves(index) {

      let original = this.boardSpaces[index]
      let opX = original.x
      let opY = original.y
      let pieceStats = {}

      if (original.piece === 'elf' || original.piece === 'orc') {
        pieceStats.distance = 2
        pieceStats.directions = [
          this.directions[0], this.directions[2], 
          this.directions[4], this.directions[6]
        ]
      } else if (original.piece === 'knight' || original.piece === 'death-knight') {
        pieceStats.distance = 3
        pieceStats.directions = [
          this.directions[0], this.directions[1], 
          this.directions[2], this.directions[3],
          this.directions[4], this.directions[5], 
          this.directions[6], this.directions[7]
        ]
      } else if (original.piece === 'giant') {
        pieceStats.distance = 3
        pieceStats.directions = [
          this.directions[0], this.directions[1], this.directions[5], 
          this.directions[6], this.directions[7]
        ]
      } else if (original.piece === 'ogre') {
        pieceStats.distance = 3
        pieceStats.directions = [
          this.directions[1], this.directions[2], this.directions[3], 
          this.directions[4], this.directions[5]
        ]
      } else if (original.piece === 'paladin' || original.piece === 'lich') {
        pieceStats.distance = 9
        pieceStats.directions = [
          this.directions[0], this.directions[1], 
          this.directions[2], this.directions[3],
          this.directions[4], this.directions[5], 
          this.directions[6], this.directions[7]
        ]
      }

      for (let i = 0; i < pieceStats.directions.length; i++) {
        for (let j = 0; j < pieceStats.distance; j++) {
          let goX = opX + pieceStats.directions[i].moveX * (j + 1)
          let goY = opY + pieceStats.directions[i].moveY * (j + 1)
          let goIndex = goX + 10 * goY
          if (goX >= 0 && goY >= 0 
          && goX <= 9 && goY <= 9 
          && this.boardSpaces[goIndex].team !== original.team) {
            if (j === 0) {
              this.availableMoves.push(goIndex)
            } else if (j !== 0) {
              if (this.boardSpaces[this.availableMoves[this.availableMoves.length - 1]].piece === 'empty') {
                this.availableMoves.push(goIndex)
              }
            }
          } else {
            j += pieceStats.distance - 1
          }
        }
      }
    console.log(this.availableMoves)
    }
  },

  created() {
    this.createBoard()
    this.createPieces()
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: rgb(5, 5, 5);
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
  background-color: #398399;
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
  background-color: #74d9f8;
}
</style>
