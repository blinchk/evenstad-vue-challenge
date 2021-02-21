<template>
    <v-card class="fill-height">
        <v-card-title>
            {{warrior.name}}
        </v-card-title>
        <v-row class="px-2 d-flex justify-center">
            <v-card elevation="5" class="d-flex justify-center">
                <v-img width="200" :src="warrior.profilePic"/>
            </v-card>
        </v-row>
        <v-row v-for="(attribute) in Object.keys(warrior.attributes)" :key="attribute" class="px-2">
            <v-col cols="5">
                {{attribute.toUpperCase()}}
            </v-col>
            <v-col cols="7">
                <v-progress-linear v-model="warrior.attributes[attribute]" color="blue-grey" height="25">
                    <template v-slot:default="{ value }">
                        <strong>{{value}}</strong>
                    </template>
                </v-progress-linear>
            </v-col>
        </v-row>
        <v-spacer/>
        <v-row class="px-2">
            <v-col cols="12">
                <v-progress-linear v-model="warrior.health" color="green" height="25">
                    <template v-slot:default="{ value }">
                        <strong>{{value}}</strong>
                    </template>
                </v-progress-linear>
            </v-col>
        </v-row>
        <v-card-actions v-if="playable">
            <v-col cols="6">
                <v-btn block elevation="2" @click="$emit('attack-melee', warrior)">Melee</v-btn>
            </v-col>
            <v-col cols="6">
                <!-- TODO
                    we need a ranged attack
                -->
                <v-btn block elevation="2">Ranged</v-btn>
            </v-col>
        </v-card-actions>
        <v-card-actions v-else>
            <v-col cols="6">
            </v-col>
        </v-card-actions>

    </v-card> 
</template>

<script>
export default {
    name: "Warrior",
    props:{
        /* Warrior model
            attributes: Object,
                key - label
                value - value
            health: Number,
            name: String,
            profilePic: String
        */
        warrior: Object,
        playable: Boolean
    }
}
</script>

<style>

</style>