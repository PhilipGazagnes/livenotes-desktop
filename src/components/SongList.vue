<template>
  <div class="index">
    <ul>
      <li
        v-for="(s, index) in songList"
        :key="index"
        :data-first-letter="s.name.charAt(0)"
      >
        <button @click="$emit('song-select', [s.id, s.name, s.artist])">
          <span>{{ s.name }}</span>
          <span>{{ s.artist }}</span>
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
      <button @click="$emit('wait')">...</button>
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
ul {
  list-style-type: none;
  margin: 0;
  padding: 20px calc(25% + 40px) 0 20px;
}
li {
  margin: 0;
  padding: 0 0 10px 0;
  display: flex;
  & > button {
    display: block;
    padding: 20px;
    text-decoration: none;
    background: rgba(255, 255, 255, 0.15);
    color: white;
    text-align: left;
    border-radius: 10px;
    cursor: pointer;
    border: none;
    &:first-child {
      flex: 1 1 auto;
    }
    & > span {
      &:nth-child(1) {
        display: block;
        font-size: 2em;
        margin-bottom: 10px;
        font-weight: bold;
      }
      &:nth-child(2) {
        opacity: 0.5;
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
  overflow: auto;
  padding: 20px;
  background: #010223;
  & > button {
    display: inline-block;
    text-transform: uppercase;
    font-weight: bold;
    font-size: 1.5em;
    line-height: 1em;
    width: 33.33%;
    padding: 16px 0;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: white;
    cursor: pointer;
  }
}
</style>
