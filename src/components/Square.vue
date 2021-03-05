<template>
    <button class="button is-light" v-on:click="click">{{label}}</button>
</template>

<script>
export default {
  name: 'Square',
  props: {
    label: String,
    id: Number
  },
  methods: {
      click: function () {
          this.$emit('click', this.id);
      }
  },
  watch: {
      label: function () {
          if(this.label === null) {
              this.label = '_';
          }
          else {
              this.label;
          }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
