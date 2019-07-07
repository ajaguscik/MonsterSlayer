<template>
    <section 
        v-if="array.length > 0"
        class="row log">
        <div class="small-12 columns">
            <ul>
                <transition-group name="slide">
                        <li 
                            v-for="action in array" 
                            :class="[{'player-turn': action.player}, {'monster-turn': !action.player}]"
                            :key="action.text"
                            >{{ action.text }}</li>
                    </transition-group>
            </ul>
        </div>
    </section>
</template>

<script>
import { eventBus } from '../main';
export default {
    data() {
        return {
            array: []
        }
    },
    created() {
        eventBus.$on('arrayWasChanged', (data) => {
            this.array = data;
        });
        console.log(this.array);
    }
}
</script>

<style>
    .slide-enter {
        opacity: 0;
        /* transform: translateY(20px); */
    }
    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }
    .slide-leave {
        
    }
    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
        position: absolute;
    }
    .slide-move {
        transition: transform 1s;
    }

    @keyframes slide-in {
        from {
            transform: translateY(-25px);
        } 
        to {
            transform: translateY(0);
        }
    }
    @keyframes slide-out {
        from {
            transform: translateY(0);
        } 
        to {
            transform: translateY(20px);
        }
    }
</style>
