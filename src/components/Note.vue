<template>
  <button class="Note" :class="classes" @click="play" @mouseenter="onEnter" @mouseleave="onLeave" @focus="onEnter" @blur="onLeave">
    <span class="Note__label" v-if="!isSharp">{{note.name}}</span>
    <span v-else></span>
  </button>
</template>

<script>

export default {
  props: {
    note: {
      type: Object,
      required: true
    }
  },

  computed: {
    isSharp() {
      return this.note.name.includes('#')
    },
    classes() {
      const c = {}
      c[this.isSharp ? 'Note--black' : 'Note--white'] = true
      return c
    }
  },

  mounted() {
    this.createAudio()

    window.addEventListener('keyup', this.onKeyUp)
  },

  beforeDestroy() {
    window.removeEventListener('keyup', this.onKeyUp)
  },

  methods: {
    onKeyUp(e) {
      if (e.keyCode === this.note.keyCode) {
        this.$el.focus()
        this.play()
      }
    },
    onEnter() {
      if(this.hovered) return
      this.hovered = true

      this.pickGif()
    },

    onLeave() {
      if(!this.hovered) return
      this.hovered = false
    },

    createAudio() {
      this.audio = document.createElement('audio')
      const note = encodeURIComponent(this.note.name)
      this.audio.src = `/assets/sounds/${note}.mp3`
    },

    pickGif() {
      this.$parent.pickGif(this.note.name)
    },

    play() {
      this.audio.currentTime = 0
      this.audio.pause()
      this.audio.play()
      
      this.hovered && this.pickGif()

      this.$parent.hideGif()
      this.$parent.showGif()
    }
  }
}
</script>

<style lang="postcss" scoped>
.Note {
  position: relative;
  outline: none;
}

.Note.Note--white {
  flex: 1;
  background-color: #fff;
  margin-right: 2px;
  margin-left: 2px;
}

.Note.Note--white .Note__label {
  position: absolute;
  bottom: -50px;
  left: 0;
  width: 100%;
  color: #fff;
  opacity: 0.4;
}

.Note:first-child {
  margin-left: 0;
  border-radius: 5px 0 0 5px;
}

.Note:last-child {
  margin-right: 0;
  border-radius: 0 5px 5px 0;
}

.Note.Note--white:focus,
.Note.Note--white:hover {
  background-color: #FFD12D;
}

.Note.Note--white:focus .Note__label,
.Note.Note--white:hover .Note__label {
  opacity: 1;
}

.Note.Note--black {
  position: relative;
  z-index: 1;
}

.Note.Note--black span{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  width: 30px;
  height: 60%;
  background-color: #000;
  border-radius: 0 0 5px 5px;
}

.Note.Note--black:focus span,
.Note.Note--black:hover span {
  background-color: #E5016F;
}

</style>