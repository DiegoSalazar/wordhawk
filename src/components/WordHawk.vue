<template>
  <div class="game">
    <span
      v-for="(letter, i) in letters"
      :key="i"
      :data-letter="letter"
      :class="activeLetter(letter)"
    >
      {{ letter }}
    </span>
  </div>
</template>

<script>
const LETTERS = 'lkashdlvkjalencfkjdhsvlnxbagsecretnujmxvkalbdfnsauytynznbcoiuyvbkiuytdtv'

export default {
  name: 'WordHawk',
  data () {
    return {
      letters: LETTERS.split(''),
      keysPressed: []
    }
  },
  computed: {
    activeLetter () {
      return letter => {
        const pressed = this.keysPressed.indexOf(letter) >= 0
        return pressed ? 'active-letter' : ''
      }
    }
  },
  methods: {
    keyPressed (e) {
      const letter = String.fromCharCode(e.keyCode)
      const i = this.keysPressed.indexOf(letter)

      if (i >= 0) {
        this.keysPressed = this.keysPressed.splice(i, 1)
      } else {
        this.keysPressed = [...this.keysPressed, letter]
      }
    },
    removePressed (letter) {
      const i = this.keysPressed.indexOf(letter)
      this.keysPressed = this.keysPressed.splice(i, 1)
    }
  },
  mounted () {
    window.addEventListener('keypress', this.keyPressed.bind(this))
  },
  destroyed () {
    window.removeEventListener('keypress', this.keyPressed)
  }
}
</script>

<style lang="scss" scoped>
.game {
  height: 400px;
  width: 400px;
  border: 1px solid black;
  padding: 10px;
  display: grid;
  grid-template-columns: repeat(9, 0.5em);
  grid-template-rows: repeat(9, 1em);
  grid-gap: 35px;

  span {
    text-transform: capitalize;
    text-align: center;
    line-height: 36px;
    border: 1px solid black;
    height: 40px;
    width: 40px;
  }

  .active-letter {
    background-color: lightblue;
    color: red;
    font-weight: bold;
  }
}
</style>
