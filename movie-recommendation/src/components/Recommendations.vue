<template>
    <div>
        <div class="row">
            <div class="col-12" style="overflow-x: hidden">
                <div class="movie-list">
                    <div v-for="(title,id) in recomendations" :key="id" class="movie-container">
                        <movie :show-ratings="false" :title="title"/>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import Movie from "@/components/Movie";

    export default {
        name: "Recommendations",
        components: {Movie},
        props: {
            ratings: {
                type: Object,
                required: true
            }
        },
        data(){
            return {
                recomendations: [],
                endpoint: "https://api.movie.cbisaillon.ca/?ratings={query}"
            }
        },
        methods: {
            ratingString(){
                return Object.entries(this.ratings).map(x => x.join(':')).join(',')
            },
            fetchRecommendations(){
                let url = this.endpoint.replace("{query}", this.ratingString())
                let self = this
                axios.get(url)
                    .then(function(response){
                        self.recomendations = response.data
                        console.log(self.recomendations)
                    })
                    .catch(function(error){
                        console.log("Error: " + error)
                    })
            }
        },
        watch: {
            ratings: {
                deep: true,
                handler(newVal){
                    this.fetchRecommendations()
                }
            }
        }
    }
</script>

<style scoped>

</style>