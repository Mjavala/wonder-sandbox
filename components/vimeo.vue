<template>
  <div v-editable="blok" class="vimeo">
    <div class="video-anchor">
      <loader v-if="!readyPlayerOne" />
      <div class="video-img-wrap">
        <div v-if="scrolled" class="video-wrap">
          <div v-if="!playing" class="video-title">{{this.title}}</div>
          <no-ssr>
            <vimeo-player
              ref="player"
              class="vimeo2"
              :video-id="videoID"
              :options="{responsive: true}"
              :controls="control"
              @ready="onReady"
              @play="onPlay"
              @pause="onPause"
              @ended="onEnded"
            />
          </no-ssr>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import loader from './loader'

export default {
  props: ['blok'],
  components: {
    loader
  },
  data () {
    return {
      video: this.blok.video,
      title: this.blok.title,
      videoID: this.blok.video,
      readyPlayerOne: false,
      control: true,
      playing: false,
      scrolled: false,
      playerReady: false
    }
  },
  watch: {
    playerReady () {
      this.showVideos()
      this.playing = false
    }
  },
  mounted () {
    window.addEventListener('scroll', () => {
      this.scrolled = true
    })
    this.playing = true
  },
  methods: {
    onReady () {
      this.playerReady = true
      this.readyPlayerOne = true
    },
    onPlay () {
      this.playing = true
    },
    onPause () {
      this.playing = false
    },
    onEnded () {
      this.playing = false
    },
    showVideos () {
      const vimeo2 = document.getElementsByClassName('vimeo2')
      const placeholders = document.getElementsByClassName('placeholder')
      for (let i = 0; i < vimeo2.length; i++) {
        vimeo2[i].classList.add('show')
      }
      for (let i = 0; i < placeholders.length; i++) {
        placeholders[i].classList.add('noShow-placeholder')
      }
    }
  }
}
</script>

<style scoped>
  @font-face { /* need eot for IE 11 */
  font-family: "Trash Regular";
  src: url("~static/fonts/trash-regular.ttf") format('truetype'),
    url("~static/fonts/trash-regular.eot#iefix") format('embedded-opentype');
  font-display: swap;
  }
  .video-img-wrap{
    position: relative;
  }
  .vimeo2{
    display: block;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
  }
  #video-wrap{
    position: relative;
  }
  .show {
    position: unset;
  }
  .noShow-placeholder{
    display: none;
  }
  .title{
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 100;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 32px;
  }
  .video-anchor{
    overflow: auto;
    background-color: black;
  }
  .video-title{
    color: white;
    font-family: "Trash Regular";
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 1000000;
    font-size: 2.5em;
    font-weight: bolder;
    transform: translate(-50%, -50%);
  }
  @media only screen and (min-width: 320px) and (max-width: 400px) {
    .video-title {
      font-size: 1.25em;
    }
  }
  @media only screen and (min-width: 401px) and (max-width: 600px) {
    .video-title {
      font-size: 1.5em;
    }
  }
  @media only screen and (min-width: 600px) and (max-width: 800px) {
    .video-title {
      font-size: 2em;
    }
  }
  @media only screen and (min-width: 1000px){
    .video-title {
      font-size: 3em;
    }
  }
</style>
