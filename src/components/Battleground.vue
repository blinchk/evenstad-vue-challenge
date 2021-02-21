<template>
    <v-container class="fill-height" fluid>
        <!-- TODO
            make hero and enemy cards same size
            preferably should conform to dynamic size set by the biggest card
        -->
        <v-row class="d-flex justify-center align-stretch">
            <v-col cols="2">
                <Warrior
                    :warrior="hero"
                    :playable="true"
                    @attack-melee="attackMelee"
                ></Warrior>
            </v-col>
            <v-col cols="2">
                <Warrior
                    :warrior="enemy"
                    :playable="false"
                ></Warrior>
            </v-col>
        </v-row>
        <v-overlay absolute :value="gameOver">
            <h1 v-if="heroWon" color="success">You won!</h1>
            <h1 v-else color="success">You lost!</h1>
            <v-btn
                color="success"
                @click="startOver"
            >
                Start Again
            </v-btn>
        </v-overlay>
    </v-container>
</template>

<script>
import Warrior from './Warrior';

export default {
    name:"Battleground",
    components: {
        Warrior
    },
    data(){
        return{
            hero: {
                name: "An Hero",
                profilePic: require('../assets/good.png'),
                health: 100,
                attributes: {
                    strength: 60,
                    accuracy: 20,
                    willpower: 60
                }
            },
            enemy: {
                name: "An Enemy",
                profilePic: require('../assets/robot.png'),
                health: 100,
                attributes: {
                    strength: 90,
                    accuracy: 40,
                    willpower: 10
                }
            }
        }
    },
    methods: {
        attackMelee(attacker){
            if(attacker === this.hero){
                /* TODO
                    implement Enemy Counter attack
                */
                this.enemy.health -= 5+(Math.random()*50);
                this.checkWinConditions();
            }
        },
        /* TODO
         Ranged Attack method
         */
        startOver(){
            this.enemy.health = 100;
            this.hero.health = 100;
        }
    },
    computed: {
        gameOver: function(){
            return this.enemy.health <= 0 || this.hero.health <= 0;
        },
        heroWon: function(){
            return this.hero.health > this.enemy.health;
        }
    }

}
</script>

<style>

</style>