<template>
  <div>
    <div v-if="win_msg_display===true">
      <div class="box">The winner is: {{ winner }} </div>
    </div>
    <div v-else>
      <div class="box">Next Player: {{ who_is_next_message }} </div>
    </div>
    <Board v-bind:squares="square_collection" v-on:click="click_manager"/>
    <History v-for="(history, index) in square_history" :key="index" v-bind:id="index" v-on:history_click="history_manager"/>
    <button class="button is-danger" v-on:click="restart">Restart Game</button>
    <p> {{turn_selected}} </p>
  </div>
</template>

<script>
import Board from './Board'
import History from './History'

export default {
  name: 'Game',
  components: {
    Board,
    History
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
      win_msg: "",
      square_history: [],
      turn_selected: []
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
      this.square_history.push(this.square_collection);
  },
  methods: {
    history_manager: function(id) {
      let temp_history = this.square_history.slice(0, id+1);
      this.square_history = [];
      this.square_history = temp_history;
      this.square_collection = [];
      this.square_collection = temp_history.slice(temp_history.length - 1);
      this.turn_selected = temp_history[temp_history.length - 1];

      this.winner = "";
      this.win_msg_display = false;
      this.win_msg = "";

      console.log("Landed in History Manager");

      // who is next ==> id%2 === 0
    },
    click_manager: function (id) {
      if(this.win_msg_display === false && this.square_collection[id].label === '_')
      {
        // Manage X and O's
        this.square_collection[id].label = this.who_is_next ? 'X' : 'O';

        // Manage History of Moves
        let newHistory = [];
        let latest = this.square_collection.slice();
        newHistory = this.square_history.concat([latest]);
        this.square_history = [];
        this.square_history = newHistory;

        // Check moves and has someone won yet?
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
      this.square_history = [];
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

      this.square_history.push(this.square_collection);
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
