<template>
    <section class="row controls">
        <div class="small-12 columns">
            <button id="attack" @click="attack(1)">ATTACK</button>
            <button id="special-attack" @click="attack(2)">SPECIAL ATTACK</button>
            <button id="heal" @click="heal">HEAL</button>
            <button id="give-up" @click="giveup">GIVE UP</button>
        </div>
    </section>
</template>

<script>
import { eventBus } from '../main';
export default {
    data() {
            return {
                scorePlayer: 100,
                scoreMonster: 100,
                array: []
            }
        },
        methods: {
            scoreChange: function (b, a) {
                return Math.floor((Math.random() * 10) + b) * a;
            },
            monsterAttack: function(a) {
                const boom = this.scoreChange(2, a);
                this.scorePlayer = this.scorePlayer - boom;
                eventBus.$emit('scorePlayerWasChanged', this.scorePlayer);
                this.array.unshift({
                    player: false,
                    text: `Monster hits You for ${boom}`
                });
                eventBus.$emit('arrayWasChanged', this.array);
                if (this.scorePlayer < 1) {
                    this.scorePlayer = 0;
                    eventBus.$emit('scorePlayerWasChanged', this.scorePlayer);
                    this.finished('Monster');
                }
            },
            attack: function(a) {
                const boom = this.scoreChange(1, a);
                this.scoreMonster = this.scoreMonster - boom;
                eventBus.$emit('scoreMonsterWasChanged', this.scoreMonster);
                this.array.unshift({
                    player: true,
                    text: `You hit Monster for ${boom}`
                });
                eventBus.$emit('arrayWasChanged', this.array);
                if (this.scoreMonster < 1) {
                    this.scoreMonster = 0;
                    eventBus.$emit('scoreMonsterWasChanged', this.scoreMonster);
                    this.finished('You');
                } else {
                    this.monsterAttack(a);
                }
            },
            heal: function() {
                const healing = this.scoreChange(5, 1);
                this.scorePlayer = this.scorePlayer + healing;
                eventBus.$emit('scorePlayerWasChanged', this.scorePlayer);
                if (this.scorePlayer > 100) {
                    this.scorePlayer = 100;
                    eventBus.$emit('scorePlayerWasChanged', this.scorePlayer);
                }
                this.array.unshift({
                    player: true,
                    text: `You recover for ${healing}`
                });
                eventBus.$emit('arrayWasChanged', this.array);
                this.monsterAttack(1);
            },
            giveup: function() {
                this.array.unshift({
                    player: true,
                    text: `You gave up`
                });
                eventBus.$emit('arrayWasChanged', this.array);
                this.$emit('componentWasChanged', 'appButtonsNewgame');
            },
            
            finished: function(winner) {
                if (confirm(`${winner} won! New game?`)) {
                    eventBus.$emit('startNewGame');
                } else {
                    this.$emit('componentWasChanged', 'appButtonsNewgame');
                }
            }
    },
    created() {
        eventBus.$on('arrayWasChanged', (data) => {
            this.array = data;
        });
        console.log(this.array);
        eventBus.$on('scorePlayerWasChanged', (data) => {
            this.scorePlayer = data;
        });
        eventBus.$on('scoreMonsterWasChanged', (data) => {
            this.scoreMonster = data;
        });
        console.log(this.scorePlayer);
        console.log(this.scoreMonster);
    }
}
</script>

<style>

</style>
