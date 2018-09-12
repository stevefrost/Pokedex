<template>
    <main>
        <h1 class="title">Pokedex</h1>
        <nav class="box">
            <div class="field">
                <label class="label">Sort by</label>
                <div class="control">
                    <div class="select">
                        <select v-model="sort" @change="sortMonsters">
                            <option value="name">Name</option>
                            <option value="num">Number</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="field">
                <label class="label">Filter</label>
                <div class="control">
                    <span
                        class="tag"
                        :class="type.toLowerCase()"
                        v-for="(type, index) in types"
                        :key="index"
                        @click="filterTypes(type, $event)">{{ type }}</span>
                </div>
            </div>
        </nav>
        <div class="columns is-multiline">
            <template v-for="monster in filteredMonsters">
                <div class="column is-one-quarter" :key="monster.id" @click="selectMonster(monster)">
                    <monster :monster="monster" />
                </div>
            </template>
            <template v-if="filteredMonsters.length == 0">
                <div class="column">No Matching Pokemon</div>
            </template>
        </div>
        <div class="modal is-active" v-if="selectedMonster">
            <div class="modal-background"></div>
            <div class="modal-content">
                <monster-details :monster="selectedMonster" @changeMonster="changeMonster" />
            </div>
            <button class="modal-close is-large" aria-label="close" @click="selectedMonster = null"></button>
        </div>
    </main>
</template>

<script>
    import axios from 'axios'
    import Monster from "./Monster";
    import MonsterDetails from "./MonsterDetails";

    export default {
        components: {
            MonsterDetails,
            Monster },
        name: "pokedex",

        data() {
            return {
                monsters: [],
                filteredMonsters: [],
                sort: 'num',
                types: [],
                filteredTypes: [],
                selectedMonster: null
            }
        },

        methods: {
            sortMonsters() {
                this.filteredMonsters.sort((a, b) => a[this.sort].localeCompare(b[this.sort]));
            },

            filterTypes(type, event) {
                event.target.classList.toggle('disabled');

                let index = this.filteredTypes.indexOf(type);

                if (index === -1) {
                    this.filteredTypes.push(type);
                } else {
                    this.filteredTypes.splice(index, 1);
                }
                this.filterMonsters();
            },

            filterMonsters() {
                this.filteredMonsters = this.monsters.filter( (monster) => {
                    for (let type of monster.type) {
                        if (this.filteredTypes.indexOf(type) > -1) {
                            return true;
                        }
                    }
                    return false;
                }, this);
            },

            selectMonster(monster) {
                this.selectedMonster = monster;
            },

            changeMonster(monsterName) {
                let monster = this.monsters.find((item) => {
                   return item.name === monsterName;
                });
                this.selectedMonster = monster;
            }
        },

        mounted() {
            axios.get('https://manage.meetupcall.com/api/pokemon').then(({ data }) => {
                this.filteredMonsters = data.pokemon;
                this.monsters = data.pokemon;

                let types = [];

                for(let pk of data.pokemon) {
                    types = [...types, ...pk.type];
                }
                this.types = [...new Set(types)];
                this.filteredTypes = [...new Set(types)];
            });

            window.addEventListener("keyup", e => {
                if (e.keyCode === 27) {
                    this.selectedMonster = null;
                }
            });
        }
    }
</script>

<style scoped>
    .tag {
        cursor: pointer;
    }
</style>