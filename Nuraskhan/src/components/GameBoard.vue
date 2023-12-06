<template>
  <div class="game-board">
    <div class="starting" v-if="!play">
      <div class="form-container">
        <label for="playerName">First Player</label>
        <input type="text" name="playerName" placeholder="Enter player name" v-model="fplayer">
      </div>
      <div class="form-container">
        <label for="playerName">Second Player</label>
        <input type="text" name="playerName" placeholder="Enter player name" v-model="splayer">
      </div>
      <button @click="fplayer!=='' && splayer!=='' ? Start() : dangerUser()">Click to Start!</button>
      <div class="danger-message">{{dangerUserInput}}</div>
    </div>
    <div v-if="play" class="playing" style="min-width: 450px">
      <div>{{ fplayer }}  {{counter[0]}}:{{counter[1]}} {{splayer}}</div>
      <div><button class="reset-button" @click="reset()">Restart</button></div>
      <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
        <div v-for="(cell, colIndex) in row" :key="colIndex" class="cell" @click="makeMove(rowIndex, colIndex)">
          {{ cell }}
        </div>
      </div>
      <div><button class="customize-button" @click="customize()">{{ custom ? "Save!" : "Customize symbols" }}</button></div>
      <div v-if="custom">
        <div class="form-container">
          <label for="playerName">{{fplayer}}:</label>
          <input type="text" name="playerName" placeholder="Enter player name" v-model="players[0]" style="width: 30px;margin-left: 5px" @input="checkForPlayers()">
        </div>
        <div class="form-container">
          <label for="playerName">{{splayer}}:</label>
          <input type="text" name="playerName" placeholder="Enter player name" v-model="players[1]" style="width: 30px;margin-left: 5px" @input="checkForPlayers()">
        </div>
        <div class="danger-message">{{dangerPlayerInput}}</div>
      </div>
      <div class="winner-message" v-if="winner!==0">{{winner==1? fplayer: splayer}} is Winner</div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      custom:false,
      fplayer:'',
      splayer:'',
      play:false,
      players:['X','O'],
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      currentPlayer: 'X',
      playable: true,
      dangerPlayerInput:"",
      dangerUserInput:'',
      winner:0,
      counter:[0,0]
    };
  },
  computed:{
    currentPlay(){
      return this.players[1];
    }
  },
  methods: {
    Start(){
      this.currentPlayer = this.currentPlay;
      this.play=true;
    },
    makeMove(row, col) {

      if(!this.playable){
        return;
      }
      if(this.board[row][col] !==''){
        return
      }
      this.board[row][col] = this.currentPlayer;
      if(this.currentPlayer ===this.players[0]){
        this.currentPlayer=this.players[1];
      }else{
        this.currentPlayer=this.players[0];
      }
      if(this.checkForWin()){
        alert("We have a winner");
        if(this.winner == 1){
          this.counter[0]++;
        }else{
          this.counter[1]++;
        }
        this.playable=false;
      }
    },
    checkForWin(){
      for(let i = 0 ; i<3;i++){
        if(this.board[i][0] ===this.board[i][1] && this.board[i][1] ===this.board[i][2] && this.board[i][2] !==''){
          if(this.board[i][0]===this.players[0]){
            this.winner=1;
          }else{
            this.winner=2;
          }
          return true;
        }
      }
      for(let j = 0 ; j<3;j++){
        if(this.board[0][j] ===this.board[1][j] && this.board[1][j] ===this.board[2][j] && this.board[0][j] !==''){
          if(this.board[0][j]===this.players[0]){
            this.winner=1;
          }else{
            this.winner=2;
          }
          return true;
        }
      }
      if(this.board[0][0] ===this.board[1][1] && this.board[1][1] ===this.board[2][2]&& this.board[2][2] !==''){
        if(this.board[0][0]===this.players[0]){
          this.winner=1;
        }else{
          this.winner=2;
        }
        return true;
      }else if(this.board[0][2] ===this.board[1][1] && this.board[1][1] ===this.board[2][0]&& this.board[2][0] !==''){
        if(this.board[2][0]===this.players[0]){
          this.winner=1;
        }else{
          this.winner=2;
        }
        return true;
      }else{
        return false;
      }
    },
    checkForPlayers(){
      if(this.players[0].length!==1 || this.players[1].length!==1){
        this.dangerPlayerInput='Enter a single character'
        this.playable=false;
      }else{
        this.dangerPlayerInput=''
        this.playable=true;
      }
    },
    reset(){
      this.winner=0;
      this.playable=true
      this.board=[
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ]
    },
    dangerUser(){
      if(this.dangerUserInput===''){
        this.dangerUserInput="Enter Correct Usernames!"
      }
    },
    customize(){
      if(this.custom===true){
        while(this.dangerPlayerInput !==''){

        }
        this.reset();
      }
      this.custom = !this.custom;
    }
  }
};
</script>

<style scoped>

.game-board {
  /* Your styling for the game board container */
}

.row {
  display: flex;
  grid-template-columns: repeat(3, 1fr);
}

.cell {
  border:1px solid #fff;
  padding:30px;
}
.cell:hover{
  background-color: #535bf2;
}
.form-container {
  padding: 20px;
  border-radius: 8px;
  border: 1px solid;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 300px;
  text-align: center;
  margin-bottom: 20px;
}
.form-container input{
  font-size: 15px;
  padding:5px;
  border-radius: 4px;
}
.starting label {
  display: block;
  font-size: 18px;
  margin-bottom: 8px;
}



.form-container {
  margin-bottom: 10px;
}

.playing label {
  display: inline-block;
  font-size: 16px;
  margin-bottom: 6px;
}

.playing input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

.row {
  display: flex;
}

.cell {
  flex: 1;
  border: 1px solid #ccc;
  padding: 20px;
  min-height:100px ;
  text-align: center;
  font-size: 60px;
  cursor: pointer;
}

.cell:hover {
  background-color: #3d3d3d;
}

.winner-message {
  font-size: 24px;
  font-weight: bold;
  color: #4caf50;
  margin-top: 10px;
}

.reset-button {
  margin-top: 20px;
  margin-bottom: 20px;
}

.customize-button {
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-top: 10px;
}

.customize-button:hover {
  background-color: #333438;
}

.customize-form label {
  width: 45%;
  margin-right: 5%;
}

.customize-form input {
  width: 50%;
  margin-left: 0;
}

.danger-message {
  color: #ff0000;
  margin-top: 10px;
}
</style>

