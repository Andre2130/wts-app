<template>
  <div class="hello">
    <h1>WeTheSauce</h1>
    <button @click="showSong">Get music</button>
    <button @click="showMedia">Get media</button>
    <div v-if="songs">
      <li v-for="song in songs" :key="song.id">
        <v-card>
          <img :src="song.jetpack_featured_media_url" alt="" width="500" height="500">
          <h5 class="text-truncate">{{song.title}}</h5>
        </v-card>
      </li>
    </div>
    <div v-if="media">
      <li v-for="song in media" :key="song.id">
        <v-card @click="playSong(song.source_url)">
          <video>
            <source :src="song.source_url">
          </video>
        </v-card>
      </li>
    </div>
  </div>
</template>

<script>
import {Howl} from 'howler';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      songs: [],
      media:{},
      img: ''
    }
  },
  methods: {
    async showSong(){
      const song = await this.$http.get('https://wethesauce.com/wp-json/wp/v2/posts?per_page=10');
      // this.songs = this.shuffleArray(song.data)
      this.songs = song.data;
      // for(let i = 0; i < songList.length; i++){
      //   this.songs.push(songList[Math.floor(Math.random()*songList.length)])
      // }
      // this.songs = song.data
     console.log(this.song)
    },
    async showMedia(){
      const song = await this.$http.get('https://wethesauce.com/wp-json/wp/v2/media?per_page=10');
      // this.songs = this.shuffleArray(song.data)
      this.media = song.data;
      // for(let i = 0; i < songList.length; i++){
      //   this.songs.push(songList[Math.floor(Math.random()*songList.length)])
      // }
      // this.songs = song.data
     console.log(this.media)
    },
    playSong(url){
      var songUrl = new Howl({
        src: [url]
      })
      console.log(songUrl);
      songUrl.play();
    },
    shuffleArray(array) {
    for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    return array;
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
  text-overflow: ellipsis;
}
a {
  color: #42b983;
}
v-card{
  height: 100px;
  width: 100px;
  display: block;
}
.title{
text-overflow: ellipsis;
}
</style>
