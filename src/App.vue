<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isplaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isplaying: false,
      songs: [
        {
          title: "Reyah Elhayah",
          artist: "Hamza Namira",
          src: require("./assets/Albumaty.Com_hmzh_nmrh_ryah_alhyat.mp3"),
        },
        {
          title: "Baad Elkalam",
          artist: "Ahmed Kamel",
          src: require("./assets/Albumaty.Com_ahmd_kaml_bad_alklam.mp3"),
        },
        {
          title: "Hygely Mawgo3",
          artist: "Tamer Ashour",
          src: require("./assets/Albumaty.Com_tamr_aashwr_hygyly_mwgwa.mp3"),
        },
        {
          title: "Mesh Saleem",
          artist: "Hamza Namira",
          src: require("./assets/Albumaty.Com_hmzh_nmrh_msh_slym.mp3"),
        },
        {
          title: "Meen Fe Dol",
          artist: "Ahmed Kamel",
          src: require("./assets/Albumaty.Com_ahmd_kaml_myn_fi_dwl.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isplaying = true;
    },
    pause() {
      this.player.pause();
      this.isplaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>
<style>
:root {
  --main-color: #19114c;
  --font-color: #474748;
  --secondary-font-color: #fff;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
  background-color: #d8d3d3fe;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: var(--font-color);
  color: var(--secondary-font-color);
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: var(--main-color);
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play,
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: var(--secondary-font-color);
  background-color: var(--main-color);
}
.next,
.prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: var(--main-color);
  border: 2px solid var(--main-color);
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: var(--font-color);
  font-weight: 700;
  font-size: 25px;
  margin-bottom: 30px;
  text-align: center;
  text-decoration: underline;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #3848b4;
}
.playlist .song.playing {
  font-size: 25px;
  color: var(--secondary-font-color);
  background-image: linear-gradient(to right, var(--main-color), #8788e9);
}
</style>
