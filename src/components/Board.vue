<template>
  <div class="box">
    <div>
        <Square v-bind:label="squares[0].label" v-bind:id ="squares[0].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[1].label" v-bind:id ="squares[1].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[2].label" v-bind:id ="squares[2].id" v-on:click="click_pass" />
    </div>
    <div>
        <Square v-bind:label="squares[3].label" v-bind:id ="squares[3].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[4].label" v-bind:id ="squares[4].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[5].label" v-bind:id ="squares[5].id" v-on:click="click_pass" />
    </div>
    <div>
        <Square v-bind:label="squares[6].label" v-bind:id ="squares[6].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[7].label" v-bind:id ="squares[7].id" v-on:click="click_pass" />
        <Square v-bind:label="squares[8].label" v-bind:id ="squares[8].id" v-on:click="click_pass" />
    </div>
    {{squares}}
  </div>
</template>

<script>
import Square from './Square'

export default {
  name: 'Board',
  props: {
    squares: Array
  },
  components: {
      Square
  },
  methods: {
      click_pass: function (id) {
          this.$emit('click', id);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
