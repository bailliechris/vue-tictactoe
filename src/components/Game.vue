<template>
  <div>
    <div v-if="win_msg_display===true">
      <div class="box">The winner is: {{ winner }} </div>
    </div>
    <div v-else>
      <div class="box">Next Player: {{ who_is_next_message }} Turn Number: {{turn_number}} </div>
    </div>
    <Board v-bind:squares="square_history[turn_number]" v-on:click="click_manager"/>
    <button class="button is-danger" v-on:click="restart">Restart Game</button>
    <History v-for="(history, index) in square_history" :key="index" v-bind:id="index" v-on:history_click="history_manager"/>
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
      who_is_next: true,
      who_is_next_message: 'X',
      winner:"",
      win_msg_display: false,
      win_msg: "",
      square_history: [],
      turn_number: 0,
    }
  },
  created: function () {
      let blank = function() {
          let blank_grid = [];

          for(let i=0; i<9; i++) {
          let data = {
            id: i,
            label: '_'
          }

          blank_grid.push(data);
        }
        return blank_grid;
      } 

      this.square_history = [[...blank()]];

      this.square_collection = blank();
  },
  methods: {
    history_manager: function(id) {
      if (id === 0) {
        this.restart();
      }
      else {
        let new_history = this.square_history.slice(0, id+1);

        this.turn_number = id;

        this.square_history = [...new_history];

        this.who_is_next = id%2 ? 0 : 1;
      }
      // who is next ==> id%2 === 0
    },
    merge_turn: function (current, new_id, symbol) {
        let blank_grid = [];

        for(let i=0; i<9; i++) {
          let data = {
            id: i,
            label: '_'
          }

          blank_grid.push(data);
        }

        for(let i=0; i<9; i++) {
          blank_grid[i].id = current[i].id;
          blank_grid[i].label = current[i].label;
        }

        blank_grid[new_id].label = symbol;

        return blank_grid;      
    },
    click_manager: function (id) {
      if(this.win_msg_display === false && this.square_history[this.turn_number][id].label === '_')
      {
        // Copy History
        const history = [...this.square_history];

        // Get previous turn
        const current = [...history[this.turn_number]];
       
        // Create next turn
        let next = this.merge_turn(current, id, this.who_is_next ? 'X' : 'O');

        // Add latest turn to new history
        this.square_history = [...history, next];

        // Increase turn counter
        this.turn_number++;

        // Check moves and has someone won yet?
        this.who_is_next_message = !this.who_is_next ? 'X' : 'O';
        this.who_is_next = !this.who_is_next;
        this.winner = this.calculate_winner(current);

        if(this.winner === 'O' || this.winner === 'X') {
          this.win_msg_display = true;
          this.win_msg = this.winner;
        }
      }
    },
    restart: function () {
      let blank = function() {
          let blank_grid = [];

          for(let i=0; i<9; i++) {
          let data = {
            id: i,
            label: '_'
          }

          blank_grid.push(data);
        }
        return blank_grid;
      } 

      this.square_history = [[...blank()]];

      this.turn_number = 0;
      this.winner = '';
      this.who_is_next = true;
      this.who_is_next_message = 'X';
      this.winner = "";
      this.win_msg_display = false;
      this.win_msg = "";
      this.win_test = "";
    },
    calculate_winner: function (squares) {
      // Data format of square_collection doesn't match lines yet - needs adjusting!
      //const squares = this.square_history[this.turn_number].slice();
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
