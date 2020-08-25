<template>
  <div v-text="hackerText">
  </div>
</template>

<script>
export default {
  name: 'VueHackerText',
  data() {
    return {
      encoded: [],
      encodedRemaining: [],
      speedValue: 20,
      decodeInterval: null,
      availableCharacters: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ~`!@#$%^&*()_+-={}[]:";\'<>?,./|\\'
    }
  },
  props: {
      text: {
          type: String,
          required: true
      },
      speed: {
          type: String,
          default: 'medium',
          validator(speed) {
              return ["slow", "medium", "fast"].indexOf(speed) !== -1
          }
      },
      mode: {
          type: String,
          default: 'hacker',
          validator(mode) {
              return ["hacker", "type", "stars"].indexOf(mode) !== -1
          }
      },
      probability: {
        type: Number,
        default: 0.1,
        validator(probability) {
          return (probability >= 0.01) && (probability < 1)
        }
      }
  },
  computed: {
    hackerText() {
      return this.encoded.join('')
    }
  },
  watch: {
    hackerText: function(val) {
      if (this.text === val) {
        clearInterval(this.decodeInterval)
      }
    }
  },
  methods: {
    encodeHacker() {
      return [...Array(this.text.length)].map(() => this.availableCharacters[Math.floor(Math.random() * this.availableCharacters.length)])
    },
    decodeHacker() {
      // Get a random position of encoded
      let charactersRemaining = Object.keys(this.encodedRemaining)
      let randPos = charactersRemaining[Math.floor(Math.random() * charactersRemaining.length)]
      
      // There is a 5% chance of selecting the correct character.  This is to help prevent the decoder for running too long.
      let char = (Math.random() < this.probability) ? this.text[randPos] : this.availableCharacters[Math.floor(Math.random() * this.availableCharacters.length)]



      this.$set(this.encoded, randPos, char)

      if (this.text[randPos] === char) {
        delete this.encodedRemaining[randPos]
      }
    }
  },
  created() {
    this.encoded = this.encodeHacker()
    this.encodedRemaining = JSON.parse(JSON.stringify(this.encoded))

    switch (this.speed) {
      case 'fast':
        this.speedValue = 5
        break;
      case 'medium':
        this.speedValue = 20
        break;
      case 'slow':
        this.speedValue = 50
        break;
    }
  },
  mounted() {
    this.decodeInterval = setInterval(() => {
      this.decodeHacker()
    }, this.speedValue)
  }
}
</script>

<style scoped>

</style>
