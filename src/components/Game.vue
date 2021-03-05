<template>
  <div>
    <div v-if="win_msg_display===true">
      <div class="box">The winner is: {{ winner }} </div>
    </div>
    <div v-else>
      <div class="box">Next Player: {{ who_is_next_message }} </div>
    </div>
    <Board v-bind:squares="square_collection" v-on:click="click_manager"/>
    <button class="button is-danger" v-on:click="restart">Restart Game</button>
  </div>
</template>

<script>
import Board from './Board'

export default {
  name: 'Game',
  components: {
    Board
  },
  props: {

  },
  data() {
    return {
      square_collection: [],
      who_is_next: true,
      who_is_next_message: 'X',
      winner:"",
      win_msg_display: false,
      win_msg: ""
    }
  },
  mounted: function () {
      for(let i=0; i<9; i++) {
        let data = {
          id: i,
          label: '_'
        }

        this.square_collection.push(data);
      } 
  },
  methods: {
    click_manager: function (id) {
      if(this.win_msg_display === false && this.square_collection[id].label === '_')
      {
        this.square_collection[id].label = this.who_is_next ? 'X' : 'O';
        this.who_is_next_message = !this.who_is_next ? 'X' : 'O';
        this.who_is_next = !this.who_is_next;
        this.winner = this.calculate_winner()

        if(this.winner === 'O' || this.winner === 'X') {
          this.win_msg_display = true;
          this.win_msg = this.winner;
        }
      }
    },
    restart: function () {
      this.square_collection = [];
      for(let i=0; i<9; i++) {
        let data = {
          id: i,
          label: '_'
        }

        this.square_collection.push(data);
      } 

      this.winner = '';
      this.who_is_next = true;
      this.who_is_next_message = 'X';
      this.winner = "";
      this.win_msg_display = false;
      this.win_msg = "";
      this.win_test = "";
    },
    calculate_winner: function () {
      // Data format of square_collection doesn't match lines yet - needs adjusting!
      const squares = this.square_collection.slice();
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];

      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a].label && squares[a].label === squares[b].label && squares[a].label === squares[c].label) {
          return squares[a].label;
        }
      }
      return false;
    } 
  }
}
</script>

<style>

</style>
