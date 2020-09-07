<template>
  <div id="app">
    <header>
      <h2>Anime Musiq</h2>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} -
          <span>{{ current.artist }}</span>
        </h2>
        <img :src="current.imgsrc" class="musicpic" />
        <div class="control">
          <button class="prev" @click="prev">
            <i class="fa fa-backward" style="font-size:18px"></i>
          </button>
          <button class="play" v-if="!isPlaying" @click="play">
            <i class="fa fa-play" style="font-size:22px"></i>
          </button>
          <button class="pause" v-else @click="pause">
            <i class="fa fa-pause" style="font-size:22px"></i>
          </button>
          <button class="next" @click="next">
            <i class="fa fa-forward" style="font-size:18px"></i>
          </button>
        </div>
      </section>
      <section class="playlist">
        <h3>My Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song-Playing' : 'song'"
        >{{ song.title }}</button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "My hero academia",
          artist: "Unknown",
          src: require("./assets/Boku no Hero Academia Opening 1 - THE DAY.mp3"),
          imgsrc: require("./assets/mha.jpg")
        },
        {
          title: "Mob psycho",
          artist: "Unknown",
          src: require("./assets/Mob Psycho 100 Opening - 99.mp3"),
          imgsrc: require("./assets/mob.jpg")
        },
        {
          title: "Full metal alchemist",
          artist: "Unknown",
          src: require("./assets/Fullmetal Alchemist Brotherhood Opening 1 - Again.mp3"),
          imgsrc: require("./assets/fmab.jpg")
        },
        // {
        //   title: "Death note",
        //   artist: "Unknown",
        //   src: require("./assets/Death Note Opening 2 - Whats Up People!.mp3")
        // },
        {
          title: "Gintama",
          artist: "Unknown",
          src: require("./assets/pray Tommy Heavenly 6.mp3"),
          imgsrc: require("./assets/gint.jpg")
        }
      ],
      player: new Audio()
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
        function() {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
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
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body,
html {
  font-family: sans-serif;
  height: 100%;
  width: 100%;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #111;
  color: #fff;
  padding: 15px;
  width: 100%;
}
.player {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
.song-title {
  position: absolute;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  width: 100%;
  text-align: center;
  height: 60px;
  background: lightskyblue;
  filter: blur(2);
}

.musicpic {
  width: 90%;
  height: 329px;
  margin-top: 20px;
}
.control {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  margin-top: 15px;
}
.control button {
  background: rgb(255, 42, 42);
  outline: none;
  border: none;
  width: 70px;
  height: 40px;
  border-radius: 10px;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.control button:nth-child(1) {
  width: 45px;
  height: 30px;
  background: transparent;
  color: black;
}
.control button:nth-child(3) {
  width: 45px;
  height: 30px;
  background: transparent;
  color: black;
}
.playlist {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
  flex-direction: row;
  width: 100%;
  height: auto;
  margin-top: 20px;
}

.playlist h3 {
  font-size: 2.6em;
  width: 100%;
  text-align: center;
}
.playlist button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
  width: 80px;
  height: 80px;
  padding: 10px;
  border-radius: 50%;
  margin-top: 15px;
}
.playlist .song {
  background: transparent;
  color: black;
  border: 1px solid black;
  outline: none;
}
.playlist .song-Playing {
  background: linear-gradient(to right, #cc2e5d, #ff5858);
  color: white;
  outline: none;
  font-weight: 900;
  font-size: 11px;
}
</style>
