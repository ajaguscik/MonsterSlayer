<template>
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">YOU</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width: scorePlayer + '%'}">
                {{ scorePlayer }}
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;" :style="{width: scoreMonster + '%'}">
                {{ scoreMonster }}
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import { eventBus } from '../main';
window.ev = eventBus;
export default {
    data() {
        return {
            scorePlayer: 100,
            scoreMonster: 100
        }
    },
    created() {
        eventBus.$on('scorePlayerWasChanged', (data) => {
            this.scorePlayer = data;
        });
        eventBus.$on('scoreMonsterWasChanged', (data) => {
            this.scoreMonster = data;
        });
    }
}
</script>

<style>

</style>
