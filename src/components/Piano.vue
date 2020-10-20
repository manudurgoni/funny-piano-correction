<template>
  <div class="Piano-container absolute left-0 w-full bottom-0 mb-16 px-6">
    <div class="video-container mx-auto my-16 relative">
      <img class="hidden" :src="nextGif" />
      <img class="video absolute object-cover top-0 left-0 w-full h-full" v-show="isShowGif" :src="currentGif"/>
    </div>
    <div class="Piano flex justify-around relative mx-auto">
      <note v-for="n in notes" :note="n" :key="n.name"></note>
    </div>
  </div>
</template>

<script>
import { GiphyFetch } from '@giphy/js-fetch-api'
import Note from '@/components/Note'
export default {
  components: {
    Note
  },
  data() {
    return {
      notes: [{
        name: 'C',
        keyCode: 81
      },{
        name: 'C#',
        keyCode: 90
      },{
        name: 'D',
        keyCode: 83
      },{
        name: 'D#',
        keyCode: 69
      },{
        name: 'E',
        keyCode: 68
      },{
        name: 'F',
        keyCode: 70
      },{
        name: 'F#',
        keyCode: 84
      },{
        name: 'G',
        keyCode: 71
      },{
        name: 'G#',
        keyCode: 89
      },{
        name: 'A',
        keyCode: 72
      },{
        name: 'A#',
        keyCode: 85
      },{
        name: 'B',
        keyCode: 74
      },],
      nextGif: null,
      currentGif: null,
      isShowGif: false
    }
  },
  
  created() {
    this.getGifs()
  },

  methods: {
    async getGifs() {
      const gf = new GiphyFetch(process.env.VUE_APP_GIPHY)
      const { data: gifs } = await gf.emoji({ limit: 97 })
      this.gifs = gifs.map((i) => i.images['fixed_width'].webp)

      // for (const n of this.notes) {
      //   const { data: gifs } = await gf.emoji({ limit: 97 })
      //   this.gifs[n] = gifs.map((i) => i.images['downsized'].url)
      // }
      // console.log(this.gifs)
    },

    hideGif() {
      this.isShowGif = false
    },

    showGif() {
      this.isShowGif = true
      this.currentGif = this.nextGif
    },

    pickGif(n) {
      if(!this.gifs) return
      this.nextGif = this.gifs[this.randomInteger(0, 97)]

      if (this.nextGif === this.currentGif) {
        this.pickGif(n)
      }
    },

    removeGif() {
      this.nextGif = null
      this.currentGif = null
    },

    randomInteger(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
  }
}
</script>

<style lang="postcss" scoped>
.Piano {
  width: 100%;
  height: 189px;
  background-color: #000;
  padding: 8px;
  border-radius: 12px;
}

@screen md {

  .Piano {
    width: 530px;
    height: 276px;
    padding: 12px;
  }
}

.video-container {
  width: 200px;
  height: 200px;
  display: block;
  overflow: hidden;
}

.video-container .video {
  /* mix-blend-mode: multiply; */
}

</style>