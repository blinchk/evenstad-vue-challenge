<template>
  <v-container class="fill-height" fluid>
    <v-row class="d-flex justify-center">
      <v-col cols="3">
        <Warrior
            :playable="true"
            :warrior="hero"
            @attack="attack">
        </Warrior>
      </v-col>
      <v-col class="align-self-center" cols="1">
        <v-card class="align-center">
          <v-card-title class="justify-center" primary-title>{{ this.hero.score }}:{{ this.enemy.score }}</v-card-title>
        </v-card>
      </v-col>
      <v-col cols="3">
        <Warrior
            :playable="false"
            :warrior="enemy">
        </Warrior>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :timeout="timeout" absolute>
      <v-icon color="success">mdi-sword</v-icon> <strong>Dealt damage:</strong> {{ Math.round(this.hero.last_damage) }} HP to An Enemy.<br>
      <v-icon color="error">mdi-water-alert</v-icon> <strong>Received damage:</strong> {{ Math.round(this.enemy.last_damage) }} HP from An Enemy.
    </v-snackbar>
    <v-overlay :value="gameOver" absolute class="light">
      <div v-if="heroWon">
        <h1>You won!</h1>
        <v-btn
            color="success"
            @click="startOver">
          Start Again
        </v-btn>
      </div>
      <div v-else>
        <h1>You lost!</h1>
        <v-btn
            color="error"
            @click="startOver">
          Start Again
        </v-btn>
      </div>
      <v-checkbox v-model="randomValues" label="With random attributes"/>
    </v-overlay>
  </v-container>
</template>

<script>
import Warrior from './Warrior';

export default {
  name: "Battleground",
  components: {
    Warrior
  },
  data() {
    return {
      hero: {
        name: "An Hero",
        health: 100,
        profilePic: require('../assets/good.png'),
        attributes: {
          strength: 60,
          accuracy: 20,
          willpower: 60
        },
        score: 0,
        last_damage: 0
      },
      enemy: {
        name: "An Enemy",
        profilePic: require('../assets/robot.png'),
        health: 100,
        attributes: {
          strength: 90,
          accuracy: 40,
          willpower: 10
        },
        score: 0,
        last_damage: 0
      },
      randomValues: true,
      snackbar: false,
      timeout: 1500,
      message: null
    }
  },
  methods: {
    bringToPositive(number) {
      if (number < 0) {
        return 0
      }
      return number
    },
    calculateEnemyAttack() {
      let attackType = Math.floor(Math.random() * 2);
      if (attackType === 1) {
        this.attack(this.enemy, true);
      } else {
        this.attack(this.enemy, false);
      }
    },
    calculateDamage(attacker, range) {
      let attackTypeAttribute = range === true ? attacker.attributes.accuracy : attacker.attributes.strength;
      return attackTypeAttribute * 0.1 + (Math.random() * 25) + attacker.attributes.willpower * 0.1;
    },
    attack(attacker, range) {
      if (attacker === this.hero) {
        attacker.last_damage = this.calculateDamage(attacker, range);
        this.enemy.health -= attacker.last_damage;
        this.enemy.health = this.bringToPositive(this.enemy.health);
        this.calculateEnemyAttack();
        this.iterateWinnersScore();
        this.snackbar = true;
      } else if (attacker === this.enemy) {
        attacker.last_damage = this.calculateDamage(attacker, range);
        this.hero.health -= attacker.last_damage;
        this.hero.health = this.bringToPositive(this.hero.health);
      }
    },
    initalizeRandomAttributes() {
      if (this.randomValues === true) {
        for(let key in this.enemy.attributes) {
          this.hero.attributes[key] = Math.floor(Math.random() * 100);
          this.enemy.attributes[key] = Math.floor(Math.random() * 100);
        }
      }
    },
    startOver() {
      this.enemy.health = 100;
      this.hero.health = 100;

    },
    iterateWinnersScore() {
      if (this.gameOver) {
        if (this.heroWon) {
          this.hero.score += 1;
        } else {
          this.enemy.score += 1;
        }
      }
    }
  },
  computed: {
    gameOver: function () {
      return this.enemy.health <= 0 || this.hero.health <= 0;
    },
    heroWon: function () {
      return this.hero.health > this.enemy.health;
    }
  }

}
</script>
