<template>
    <div>
        <div class="row">
            <span @click="moveLeft()" class="col-1 arrow-btn"><i class="fas fa-chevron-left"></i></span>
            <div class="col-10" style="overflow-x: hidden">
                <div class="movie-list">
                    <div v-for="(title,id) in movies" :key="id" class="movie-container">
                        <movie @rated="rated(id, $event)" :title="title"/>
                    </div>
                </div>
            </div>
            <span @click="moveRight()" class="col-1 arrow-btn"><i class="fas fa-chevron-right"></i></span>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import $ from 'jquery'
    import Movie from "@/components/Movie";

    export default {
        name: "MovieList",
        components: {Movie},
        data: function(){
            return {
                endpoint: "https://api.movie.cbisaillon.ca/list?page={page}",
                page: 1,
                xPos: 0,
                delta:220,
                movies: {},
                ratings: {}
            }
        },
        computed:{
            _(){
                return this._
            }
        },
        methods: {
            retrieveMovies(){
                let url = this.endpoint.replace('{page}', this.page)
                let self = this
                axios.get(url)
                    .then(function(response){
                        self.movies = response.data
                    })
                    .catch(function(error){
                        console.log(error)
                    })
            },
            moveRight(){
                if(this.xPos < Object.keys(this.movies).length - 4) {
                    this.xPos += 1
                }else{
                    //Return to begining
                    this.xPos = 0
                    this.page += 1
                    this.retrieveMovies()
                }

                this.doMove()
            },
            moveLeft(){
                if(this.xPos >= 1){
                    this.xPos -= 1
                }

                this.doMove()
            },
            doMove(){
                let pos = this.xPos * this.delta

                $('.movie-list').stop(true, false).animate({
                    right: pos
                })
            },
            rated(movieId, rating){
                console.log("Rated " + movieId)
                this.$set(this.ratings, movieId, rating)

                this.moveRight()
                this.$emit('ratingsChanged', this.ratings)
            }
        },
        mounted(){
            this.retrieveMovies()
        }
    }
</script>

<style scoped>
    .movie-list{
        width:800%;
        margin:auto;
        position:relative
    }

    .arrow-btn{
        font-size: 3em;
        text-align: center;
        cursor: pointer;
    }

</style>