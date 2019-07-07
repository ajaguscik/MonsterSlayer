<template>
    <section class="row controls">
        <div class="small-12 columns">
            <button id="start-game" @click="startNewGame">START NEW GAME</button>
        </div>
    </section>
</template>

<script>
import { eventBus } from '../main';

export default {
    methods: {
        startNewGame: function() {
                this.array = [];
                this.scorePlayer = 100;
                this.scoreMonster = 100;
                eventBus.$emit('arrayWasChanged', this.array);
                eventBus.$emit('scorePlayerWasChanged', this.scorePlayer);
                eventBus.$emit('scoreMonsterWasChanged', this.scoreMonster);
                this.$emit('componentWasChanged', 'appButtonsAttack');
        } 
    },
    created() {
        eventBus.$on('startNewGame', () => {
            this.startNewGame();
        });
    }
}
</script>

<style>

</style>
