<template>
    <div class="movie">
        <div v-if="showRatings" class="rating-buttons mb-2">
            <div class="w-100 d-flex mx-auto" data-toggle="buttons">
                <button @click="rate(1)" class="btn btn-secondary rate-button">1</button>
                <button @click="rate(2)" class="btn btn-secondary rate-button">2</button>
                <button @click="rate(3)" class="btn btn-secondary rate-button">3</button>
                <button @click="rate(4)" class="btn btn-secondary rate-button">4</button>
                <button @click="rate(5)" class="btn btn-secondary rate-button">5</button>
            </div>
        </div>
        <img class="poster" :src="posterUrl"/>
        <p class="title">{{title}}</p>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "Movie",
        props: {
            title: {
                type: String,
                required: true
            },
            showRatings: {
                type: Boolean,
                default: true
            }
        },
        data: function(){
            return {
                apiKey: "15d2ea6d0dc1d476efbca3eba2b9bbfb",
                endpoint: "https://api.themoviedb.org/3/search/movie?api_key={APIKEY}&query={NAME}",
                posterEndpoint: "http://image.tmdb.org/t/p/w500",
                posterUrl: ""
            }
        },
        methods: {
            getImageUrl(){
                let url = this.endpoint.replace('{APIKEY}', this.apiKey)
                url = url.replace("{NAME}", this.title.substr(0, this.title.lastIndexOf(" ")))

                let self = this
                axios.get(url)
                    .then(function(response){
                        if(response.data.results[0]) {
                            let posterPath = response.data.results[0].poster_path
                            self.posterUrl =  self.posterEndpoint + posterPath
                        }
                    })
                    .catch(function(error){
                        console.log("Error: " + error)
                    })
            },
            rate(rating){
                this.$emit('rated', rating)
            }
        },
        mounted() {
            this.getImageUrl()
        },
        watch: {
            title: {
                handler(newVal){
                    this.getImageUrl()
                }
            }
        }
    }
</script>

<style scoped>
    .movie{
        display:inline-block;
    }
    .poster{
        width:100%;
    }
    .title{
        font-weight:700;
        font-size:1.1em;
    }
    .rate-button{
        flex-grow:20;
        margin-right:5px;
    }
</style>