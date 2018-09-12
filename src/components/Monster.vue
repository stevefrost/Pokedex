<template>
    <article class="box monster animate">
        <div class="card-image has-text-centered">
            <img :src="monster.img" />
        </div>
        <h3 class="subtitle is-6">#{{ monster.num }}</h3>
        <h2 class="title is-capitalized">{{ monster.name }}</h2>
        <div>
            <p><strong>Height:</strong> {{ monster.height }}</p>
            <p><strong>Weight:</strong> {{ monster.weight }}</p>
        </div>
        <div class="abilities">
            <span v-for="(type, i) in monster.type" class="tag" :class="type.toLowerCase()" :key="i">{{ type }}</span>
        </div>
    </article>
</template>

<script>
    export default {
        name: "monster",

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

    .monster {
        cursor: pointer;
    }
</style>