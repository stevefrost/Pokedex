<template>
    <article class="box monster">
        <div class="card-image has-text-centered">
            <img :src="monster.img" />
        </div>
        <div class="columns">
            <div class="column">
                <h3 class="subtitle is-6">#{{ monster.num }}</h3>
                <h2 class="title is-capitalized">{{ monster.name }}</h2>
                <div>
                    <p><strong>Height:</strong> {{ monster.height }}</p>
                    <p><strong>Weight:</strong> {{ monster.weight }}</p>
                </div>
            </div>
            <div class="column">
                <div class="abilities">
                    <h4>Type</h4>
                    <span v-for="(type, i) in monster.type" class="tag" :class="type.toLowerCase()" :key="i">{{ type }}</span>
                </div>
                <h4>Weaknesses</h4>
                <div class="weaknesses">
                    <span v-for="(weakness, i) in monster.weaknesses" class="tag" :class="weakness.toLowerCase()" :key="i">{{ weakness }}</span>
                </div>
            </div>
        </div>
        <div class="columns">
            <div class="column">
                <div v-if="monster.prev_evolution" class="has-text-left">
                    <h5>Previous Evolution</h5>
                    <button
                        class="button is-primary is-pulled-left"
                        @click="$emit('changeMonster', monster.prev_evolution[monster.prev_evolution.length-1].name)">{{ monster.prev_evolution[monster.prev_evolution.length-1].name }}</button>
                </div>
            </div>
            <div class="column">
                <div v-if="monster.next_evolution" class="has-text-right">
                    <h5>Next Evolution</h5>
                    <button
                        class="button is-primary is-pulled-right"
                        @click="$emit('changeMonster', monster.next_evolution[0].name)">{{ monster.next_evolution[0].name }}</button>
                </div>
            </div>
        </div>
    </article>
</template>

<script>
    export default {
        name: "monsterDetails",

        props: {
            monster: {
                type: Object,
                required: true
            }
        },

        methods: {

        }
    }
</script>

<style scoped lang="scss">
    .subtitle {
        color: #7a7a7a;
    }
</style>