<template>
    <div @click="onClick" class="game-piece" :class="`game-piece--${selection}`">
        <div class="game-piece__value">
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            selection: {
                type: String,
                required: true,
            },
        },
        methods: {
            onClick() {
                this.$emit('click', this.selection);
            }
        }
    }
</script>