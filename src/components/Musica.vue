<template>
    <main class="base" alt="Imagen de Album">
      <section class="player">
        <div class="img-album"> 
         <img class="img-portada" v-bind:src="current.img" alt="Imagen de Album" width="200px" height="200px">
        </div>
        <h2 class="song-title">{{ current.title }}</h2>
        <span class="artist-title">{{ current.artist }}</span>
        <div class="tiempo">
        <!--<span class="tiempo-inicial">00:00</span>
        <input class="rango_duracion" type="range" id="file" min="1" max="100" value="0" onchange="cambiarDuracion" />
         <span class="tiempo-final"></span>-->
         <button class="mute" @click="mute"><i :class="[silenciar ? iconoSilencio : iconoSonido]"></i></button>
        </div>
        <div class="control">
          <button class="prev" @click="prev"><i class="fa-solid fa-angle-left"></i></button>
          <button class="play" v-if="!isPlaying" @click="play"><i class="fa-solid fa-play"></i></button>
          <button class="pause" v-else @click="pause"><i class="fa-solid fa-pause"></i></button>
          <button class="next" @click="next"><i class="fa-solid fa-angle-right"></i></button>
        </div>
      </section>
      <hr class="division-movil">
      <section class="playlist">
        <h3 class="list-title">Lista de Canciones</h3>
        <button class="canciones-lista" v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
</template>
<script>
import musica from '../assets/canciones/canciones.json'
export default {
  name: 'Musica',
  data (){
    return {
     current: {},
     index: 0,
     isPlaying: false,
     songs:[
       {
         title: 'Bulletproof...I wish i was',
         artist: 'RadioHead',
         src: require('../assets/Radiohead - Bulletproof...I wish i was.mp3'),
         img: require('../assets/img/album-TheBends.jpg')
       },
       {
         title: 'Scatterbrain',
         artist: 'RadioHead',
         src: require('../assets/Radiohead - Scatterbrain.mp3'),
         img: require('../assets/img/album-HallToTheThief.jpg')
       },
       {
         title: 'Paranoid Android',
         artist: 'RadioHead',
         src: require('../assets/Radiohead - Paranoid Android.mp3'),
         img: require('../assets/img/album-OkComputer.jpg')
       },
       {
         title: 'Exit Music (For a Film)',
         artist: 'RadioHead',
         src: require('../assets/Radiohead - Exit Music (For A Film).mp3'),
         img: require('../assets/img/album-OkComputer.jpg')
       },
       {
         title: 'Nude',
         artist: 'RadioHead',
         src: require('../assets/Radiohead - Nude.mp3'),
         img: require('../assets/img/album-InRainbows.jpg')
       }
     ],
     canciones: musica,
     player: new Audio(),
     silenciar: false,
     iconoSilencio: "fa-solid fa-volume-xmark",
     iconoSonido: "fa-solid fa-volume-high",
    }
  },
  methods:{
    play: function(song){
      if(typeof song.src != 'undefined'){
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function() {
        this.index++
        if(this.index > this.songs.length - 1){
          this.index = 0;
        }
      this.current = this.songs[this.index];
      this.play(this.current);

      }.bind(this))
      this.isPlaying = true
    },
    pause: function(){
      this.player.pause()
      this.isPlaying = false;
    },
    next: function(){
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev: function(){
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length -1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
      this.cambiarFondo(this.current.img)
    },
    mute: function(){
      this.player.volume !== 0 ? this.player.volume = 0 : this.player.volume = 1;
      this.silenciar = !this.silenciar;
    },
    cambiarFondo: function(img){
      this.background = img;
    }
  },
  created(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Abel&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Abel', sans-serif;
  transition: 0.3s;
  background: linear-gradient(to bottom right, #181818, rgb(99, 99, 99)) no-repeat fixed;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

main{
  width: 100%;
  min-height: 600px;
  background: url('../assets/img/album-TheBends.jpg');
  background-size: cover;
  margin: 0 auto;
  margin-top: 30px;
  margin-bottom: 30px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  position: relative;
}

main::after{
  content: '';
  width: 100%;
  height: 100%;
  backdrop-filter: blur(50px);
  position: absolute;
  border-radius: 10px;
}
.player{
  width: 70%;
  min-width: 400px;
  text-align: center;
  margin-top: 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.volumen{
  color: #fff;
  margin: 10px;
  display: flex;
}

.volumen i{
  margin-left: 5px;
  margin-right: 5px;
}

.rango-volumen{
  width: 200px;
}

.img-album{
  width: 50%;
  border-radius: 10px;
}

.img-portada{
  border-radius: 10px;
}

.song-title, .list-title{
  padding: 10px 10px 0px 10px;
  color: #fff;
  height: 50px;
}

.artist-title{
    color: #fff;
    padding-bottom: 10px;
}

.tiempo{
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  padding: 5px;
}

.control{
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: space-evenly;
}

.mute{
  background: none;
  border: none;
  color: #fff;
  font-size: 20px;
  margin: 10px;
}

.prev, .play, .pause, .next{
    width: 50px;
    cursor: pointer;
    background: none;
    border: none;
    color: #fff;
    font-size: 40px;
    transition: 0.3s;
}
.prev:hover, .play:hover, .pause:hover, .next:hover{
    color: #b91ae0;
}

.play, .pause{
  background-color:#000;
  border-radius: 50px;
  text-align: center;
  font-size: 25px;
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.4);
}

.division-movil{
  width: 100%;
  height: 1px;
  display: none;
  border: none;
  margin-top: 40px;
  background-color: #fff;
  z-index: 1000;
}

.playlist{
  width: 100%;
  text-align: center;
  margin:40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
    z-index: 1000;
}

.canciones-lista{
  width: 100%;
  height: 50px;
  background: none;
  border: none;
  color: #fff;
  cursor: pointer;
  transition: 0.5s;
}

.canciones-lista:hover{
  color: #b91ae0;
}

@media (max-width: 900px){
    main{
      width: 100%;
      flex-direction: column;
    }
    .division-movil{
      display: block;
    }
}
@media (max-width: 410px){
    main{
      width: 100%;
      height: 100%;
      font-size: 12px;
      margin: 0;
    }
}
</style>
