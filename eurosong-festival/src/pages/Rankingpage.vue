<template>
    <div>
        <button @click="goToPage('home')">
            Go to home
        </button>
        <h1>
            Ranking
        </h1>
        <iframe src=https://open.spotify.com/embed/track/4NsPgRYUdHu2Q5JRNgXYU5 width="300" height="80" frameborder="0" allowtransparency="true" allow="encrypted-media"></iframe>

                <Carousel
                
            :items="songs"
        />

    </div>
</template>


<script>
    // components
    import Carousel from "../components/Car.vue";

    // export
    export default {
        name: "Rankingpage",
        components: {
            Carousel
        },
        data() {
            return {
                songs: []
            }
        },
        mounted() {
            this.fetchSongs();
        },
        methods: {
            // navigation method
            goToPage(page) {
                this.$emit("change-page", page);
            },

            // data methods
            fetchSongs() {
                // Get all songs
                const url = "http://webservies.be/eurosong/Songs";

                fetch(url)
                    .then((response) => {
                        return response.json();
                    })
                    .then((songs) => {
                        this.fetchArtists(songs);
                    });
            },

            fetchArtists(songs) {
                // Get all artist
                const url = "http://webservies.be/eurosong/Songs";

                fetch(url)
                    .then((response) => {
                        // response is text, so convert to json
                        return response.json();
                    })
                    .then((songs) => {
                        // loop over array songs with forEach method
                        songs.map((song) => {
                            // find the artist in an array
                            //const artist = artists.find((artist) => artist.id == song.artist);
                            const ids = songs.find((song) => song.id == song.id);
                            //log ids
                            //console.log(ids.id);
                            //console.log(song.id);


                            const urll = "http://webservies.be/eurosong/Songs/" + song.id + "/points"
                            
                            let binData = null;

                            //FETCH AND LOG POINTS
                            fetch(urll)
                               .then(result => result.json())
                               .then(data => {
                                        binData = data;
                                        //console.log("binData", binData);
                                        song.id = binData;

                                      });




                            // return the new object
                            return song;
                        });

                        //sorting
                        songs.sort(function(a, b){
                            return a.id - b.id;
                                            });




                        // change data of songs, so everything will get rerenderd;

                        this.songs = songs;
                        //var urlwinner = songs[0].spotify;
                        const urlwinner = "https://open.spotify.com/embed/track/4NsPgRYUdHu2Q5JRNgXYU5"
                        //console.log(urlwinner);
                        console.log(songs[0].spotify);

                    });
            }
        }
    }
</script>