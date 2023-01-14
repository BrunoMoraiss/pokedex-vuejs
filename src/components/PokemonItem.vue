<template>
    <div id="content">
        <div class="card">
            <div class="card-image">
                <figure>
                    <img :src="currentImg" alt="Placeholder image" />
                    <img />
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{ num }} - {{ upperCase }}</p>
                        <p class="subtitle is-6">{{ pokemon.type }}</p>
                    </div>
                </div>
                <div class="content">
                    <button @click="changeSprite" class="button is-danger is-outlined is-small is-fullwidth">Mudar Sprite</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data() {
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: "",
                frontImg: "",
                backImg: "",
            },
        };
    },
    created: async function () {
        try {
            const res = await axios.get(this.url);
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.frontImg = res.data.sprites.front_default;
            this.pokemon.backImg = res.data.sprites.back_default;
            this.currentImg = this.pokemon.frontImg
        } catch (err) {
            console.log(err);
        }
    },
    props: {
        name: String,
        url: String,
        num: Number,
    },
    computed: {
        upperCase() {
            const newName =
            this.name[0].toUpperCase() + this.name.slice(1, this.name.length);
            return newName;
        },
    },
    methods: {
        changeSprite(){
            if(this.isFront){
                this.isFront = false
                this.currentImg = this.pokemon.backImg
            }else{
                this.isFront = true
                this.currentImg = this.pokemon.frontImg
            }
        }
    }
};
</script>

<style scoped>
#content {
    width: 250px;
    height: 250px;
    margin-bottom: 12px;
}
.card{
    background-color: #A9A9A9;
}
</style>