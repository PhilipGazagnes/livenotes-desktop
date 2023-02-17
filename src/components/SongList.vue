<template>
  <div class="index">
    <ul>
      <li
        v-for="(s, index) in songList"
        :key="index"
        :data-first-letter="s.name.charAt(0)"
      >
        <button @click="$emit('song-select', s.id)">
          <span>{{ s.name }}</span>
          <span>{{ index + 1 }} ({{ s.artist }})</span>
        </button>
      </li>
    </ul>
    <div class="letters">
      <button
        v-for="(l, index) in $options.alphabet"
        :key="index"
        @click="scrollToLetter(l)"
      >
        {{ l }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  alphabet: 'abcdefghijklmnopqrstuvwxyz',
  props: {
    songList: Array,
  },
  methods: {
    scrollToLetter(letter) {
      const matches = document.querySelectorAll(
        `[data-first-letter=${letter.toUpperCase()}]`,
      );
      if (matches.length > 0) {
        const offsetTopFirstMatch = matches[0].offsetTop;
        window.scrollTo(0, offsetTopFirstMatch);
      }
    },
  },
};
</script>

<style lang="postcss">
body {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  font-weight: 400;
}
.index {
  background: white;
  h1 {
    padding: 30px 20px;
  }
}
.random {
  padding: 10px 20px;
  margin: 0 0 40px 20px;
  font-size: 1em;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
li {
  margin: 0;
  padding: 0 10px;
  border-bottom: 1px solid #eee;
  display: flex;
  &:nth-child(odd) {
    background: rgba(0, 0, 0, 0.02);
  }
  &:last-child {
    padding: 200px 0 20px 20px;
    & > button {
      padding: 15px 30px;
    }
  }
  & > button {
    display: block;
    padding: 10px;
    text-decoration: none;
    color: #333;
    &:first-child {
      flex: 1 1 auto;
    }
    & > span {
      &:nth-child(1) {
        display: block;
        font-weight: bold;
        font-size: 1.5em;
      }
      &:nth-child(2) {
        opacity: 0.5;
      }
      &.nota {
        color: red;
      }
    }
  }
}
.letters {
  position: fixed;
  height: 100%;
  width: 25%;
  top: 0;
  right: 0;
  background: #222;
  overflow: auto;
  & > button {
    display: inline-block;
    text-transform: uppercase;
    font-weight: bold;
    font-size: 1.5em;
    line-height: 1em;
    width: 50%;
    padding: 22px 0;
    &.switchscope {
      width: 100%;
      border: purple 2px solid;
      font-size: 1em;
    }
  }
  @media screen and (min-width: 768px) {
    width: 20%;
    button {
      width: 33.33%;
      padding: 16px 0;
    }
  }
}
</style>
