<template>
    <div id="app">
        <div class="container-fluid bg-dark header">
            <h1 class="text-white text-weight-bold text-center">My Music</h1>
        </div>

        <div class="row">
            <h5 class="song-name col-sm-12"><span class="titl">{{ current.title }} - {{ current.artist }}</span></h5>
        </div>
        <div class="controls">
            <button class="prev" @click="previous" :disabled="current.title==songs[0].title">Prev</button>
            <button class="play" v-if="isPlaying" @click="play">Play</button>
            <button class="pause" v-else @click="pause">Pause</button>
            <button class="next" @click="next" :disabled="current.title==songs[songs.length-1].title">Next</button>
        </div>
        <div>
            <h2 class="font-weight-bold text-center pb-2">The Playlist</h2>
            <div class="playList" v-for="(song,index) in songs" :key="song.src">
                <button class="btn" @click="getind(song)">
                    <span :class="(current.src==song.src) ? 'songPlaying' : 'justSong'">{{ index+1 }} - {{ song.title }} - {{ song.artist }}</span>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                current: {},
                isPlaying: true,
                index: 0,
                songs: [{
                        title: "A new beginning",
                        artist: "ABC",
                        src: require("./assets/anewbeginning.mp3")
                    },
                    {
                        title: "Creative minds",
                        artist: "DEF",
                        src: require("./assets/creativeminds.mp3")
                    },
                    {
                        title: "Little idea",
                        artist: "GHI",
                        src: require("./assets/littleidea.mp3")
                    },
                    {
                        title: "Summer",
                        artist: "JKL",
                        src: require("./assets/summer.mp3")
                    },
                    {
                        title: "Ukulele",
                        artist: "MNO",
                        src: require("./assets/ukulele.mp3")
                    }
                ],
                player: new Audio(),
            }
        },
        methods: {
            play(song) {
                if (typeof song.src != "undefined") {
                    this.current = song;
                    this.player.src = this.current.src;
                }
                this.player.play();
                this.isPlaying = false;

                this.player.addEventListener('ended', function() {
                    this.index++;
                    if (this.index == this.songs.length) {
                        this.index = 0;
                    }
                    this.current = this.songs[this.index];
                    this.play(this.current);
                }.bind(this));
            },
            getind(song){
                this.index = this.songs.findIndex(x=>x.src === song.src);
                this.current = this.songs[this.index];
                this.play(this.current);
            },
            pause() {
                this.player.pause();
                this.isPlaying = true;
            },
            previous() {
                this.index--;
                this.current = this.songs[this.index];
                this.play(this.current);
            },
            next() {
                this.index++;
                this.current = this.songs[this.index];
                this.play(this.current);
            }
        },
        created() {
            this.current = this.songs[this.index];
            this.player.src = this.current.src;
        }
    }
</script>

<style>
    #app {
        background: url(assets/back.jpg) no-repeat center fixed;
        -webkit-background-size: cover;
        -moz-background-size: cover;
        -o-background-size: cover;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    .header {
        min-height: 50px;
        float: left;
    }

    .song-name {
        font-weight: bolder;
        text-align: center;
        padding-top: 3em;
    }

    .titl {
        color: white;
        background-color: #cc2e5d;
        padding: .5em;
        border-radius: .5em;
    }

    .controls {
        text-align: center;
    }

    .play {
        font-size: 200%;
        color: white;
        background-color: #990000;
        margin: 1em;
        padding: .2em;
        border-radius: .2em;
        width: auto;
    }

    .play:hover {
        color: black;
        background-color: #cc6666;
    }

    .pause {
        font-size: 200%;
        background-color: #cc6666;
        margin: 1em;
        padding: .2em;
        border-radius: .2em;
        width: auto;
    }

    .prev {
        font: 200% black;
        background-color: #ff0066;
        margin: 1em;
        border-radius: .2em;
        width: auto;
    }

    .next {
        font: 200% black;
        background-color: #ff0066;
        margin: 1em;
        border-radius: .2em;
        width: auto;
    }

    .playList button {
        font-size: 110%;
        line-height: 1em;
        display: block;
        margin: 0 auto;
        font-weight: bold;
    }

    .songPlaying {
        color: white;
        background-color: #990000;
        padding-left: 1em;
        padding-right: 1em;
        border-radius: .2em;
    }
</style>