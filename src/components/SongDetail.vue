<template>
  <div class="song">
    <button
      class="close"
      @click="$emit('close-song')"
    >
      close
    </button>
    <div ref="lyrics" class="lyrics" :style="{ fontSize: `${fontSizeUser}em` }">
      <div
        v-for="(lyric, index) in songLyrics"
        :key="index"
        class="lyric"
        :data-type="lyric.type"
      >
        {{ lyric.lyric }}
      </div>
    </div>
    <div class="tools">
      <button class="toolsButton" @click="fontSize(true)">+</button>
      <button class="toolsButton" @click="fontSize(false)">-</button>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    songName: String,
    songArtist: String,
    songLyrics: Array,
  },
  data() {
    return {
      fontSizeUser: 2,
    };
  },
  methods: {
    lyric(str) {
      if (str.indexOf('***') >= 0) {
        const spl = str.split('***');
        if (spl.length > 1) {
          return `<em>${spl[1]}</em>`;
        }
      }
      if (str.indexOf('+++') >= 0) {
        const spl = str.split('+++');
        if (spl.length > 1) {
          return `<i>${spl[1]}</i>`;
        }
      }
      return str;
    },
    fontSize(increase) {
      this.fontSizeUser += 0.1 * (increase ? 1 : -1);
    },
  },
};
</script>

<style lang="postcss">
.song {
  position: fixed;
  width: 100%;
  height: 100%;
  z-index: 100;
  top: 0;
  left: 0;
  background: black;
  overflow-x: scroll;
  color: white;
}
.close {
  position: absolute;
  top: 0;
  left: 0;
}
.lyrics {
  position: relative;
  top: 30px;
  height: calc(100% - 40px);
  width: 10000px;
  padding: 0 50px;
  overflow: hidden;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  align-content: flex-start;
}
.lyric {
  width: 500px;
  font-weight: bold;
  padding: 0 80px 0.8em 0;
  line-height: 1.5em;
  color: white;
  
  &[data-type='instru'] {
    font-family: 'Courier';
    font-style: italic;
    margin-bottom: 0.8em;
    position: relative;
    background-color: blue;
    background-size: calc(100% - 80px) 100%;
    &::before {
      content: '🎸';
      font-size: 2em;
    }
    &::after {
      content: '';
      display: block;
      width: 80px;
      height: 100%;
      background: black;
      top: 0;
      right: 0;
      position: absolute;
    }
  }
  &[data-type='refrain'] {
    color: yellow;
  }
}
.tools {
  position: fixed;
  right: 10px;
  bottom: 10px;
  width: 40px;
  &Button {
    background: rgba(255, 255, 0, 0.2);
    border: 1px solid #ccc;
    border-radius: 3px;
    font-size: 20px;
    width: 100%;
    height: 40px;
    margin-top: 10px;
  }
}
</style>
