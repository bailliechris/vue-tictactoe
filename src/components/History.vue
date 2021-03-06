<template>
        <button class="button is-info" v-on:click="send_id">{{id === 0 ? "Game Start" : ("Move" + id)}}</button>
</template>

<script>
export default {
    name: 'History',
    data() {
        return {

        }
    },
    props: {
        id: Number
    },
    methods: {
        send_id: function() {
            this.$emit('history_click', this.id);
        }
    }
}
</script>