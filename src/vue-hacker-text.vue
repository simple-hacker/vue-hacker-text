<template>
  <div>
    <span v-text="hackerText"></span>
    <span v-text="typeCharacter" v-show="showTypeCharacter" class="typeCharacter"></span>
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
      typeCharacterInterval: null,
      availableCharacters: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ~`!@#$%^&*()_+-={}[]:";\'<>?,./|\\',
      decoder: null,
      showTypeCharacter: false,
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
              return ['slow', 'medium', 'fast'].indexOf(speed) !== -1
          }
      },
      mode: {
          type: String,
          default: 'hacker',
          validator(mode) {
              return ['hacker', 'stars', 'type'].indexOf(mode) !== -1
          }
      },
      probability: {
        type: Number,
        default: 0.1,
        validator(probability) {
          return (probability >= 0.005) && (probability < 1)
        }
      },
      typeCharacter: {
        type: String,
        default: '|',
        validator(char) {
          // Only one character is valid
          return char.length === 1
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
      // clearInterval when decoder has finished by compairing hackerText to the original value.
      if (this.text === val) {
        clearInterval(this.decodeInterval)
        if (this.typeCharacterInterval) {
          clearInterval(this.typeCharacterInterval)
          this.showTypeCharacter = false
        }

        // Emit decodingFinished for callback function
        this.$emit('decodeFinish')
      }
    }
  },
  methods: {
    encodeHacker() {
      return [...Array(this.text.length)].map(() => this.availableCharacters[Math.floor(Math.random() * this.availableCharacters.length)])
    },
    encodeStars() {
      return [...Array(this.text.length)].map(() => '*')
    },
    encodeType() {
      return [...Array(this.text.length)].map(() => null)
    },
    decodeRandom() {
      // Get a random position of encoded
      let charactersRemaining = Object.keys(this.encodedRemaining)
      let randPos = charactersRemaining[Math.floor(Math.random() * charactersRemaining.length)]
      
      this.changeCharacterAt(randPos)
    },
    decodeFirst() {
      // Get the first encoded character of the encodedRemaining
      let pos = Object.keys(this.encodedRemaining).shift()

      this.changeCharacterAt(pos)
    },
    changeCharacterAt(pos) {
      // Choose a random character to replace at index with a probability of choosing the correct character
      let char = (Math.random() < this.probability) ? this.text[pos] : this.availableCharacters[Math.floor(Math.random() * this.availableCharacters.length)]
      
      // Set encoded character at index to chosen character
      this.$set(this.encoded, pos, char)

      // If it's the correct character then delete index from encodedRemaining
      if (this.text[pos] === char) {
        delete this.encodedRemaining[pos]
      }
    }
  },
  created() {
    // Set decoding mode and encoded value.
    switch (this.mode) {
      case 'hacker':
        this.encoded = this.encodeHacker()
        this.decoder = this.decodeRandom
        break;
      case 'stars':
        this.encoded = this.encodeStars()
        this.decoder = this.decodeFirst
        break;
      case 'type':
        this.encoded = this.encodeType()
        this.decoder = this.decodeFirst
        break;
    }

    // Create copy of encoded.
    // encodedRemaining is used to keep a track of which character indexes have not yet been decoded.
    this.encodedRemaining = JSON.parse(JSON.stringify(this.encoded))

    // Set decoding speed
    switch (this.speed) {
      case 'fast':
        this.speedValue = 5
        break;
      case 'medium':
        this.speedValue = 25
        break;
      case 'slow':
        this.speedValue = 50
        break;
    }
  },
  mounted() {

    // Enable before decoding function callback.
    this.$emit('decodeStart')

    // Run decoder at speed interval
    this.decodeInterval = setInterval(() => {
      this.decoder()
    }, this.speedValue)

    // Alternate the visibility of the typeCharacter every 500ms
    if (this.mode === 'type') {
      this.typeCharacterInterval = setInterval(() => {
        this.showTypeCharacter = ! this.showTypeCharacter
      }, 500)
    }
  }
}
</script>

<style scoped>
  .typeCharacter {
    margin-left: 3px;
  }
</style>
