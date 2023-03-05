<template>
  <div class="song">
    <button
      class="close"
      @click="$emit('close-song')"
    />
    <div ref="lyrics" class="lyrics" :style="{ fontSize: `${fontSizeUser}em` }">
      <div
        v-for="(lyric, index) in songData.lyrics"
        :key="index"
        class="lyric"
        :data-type="lyric.type"
      >
        {{ lyric.lyric }}
      </div>
    </div>
    <div class="tools">
      <button class="toolsButton" @click="fontSize(false)">-</button>
      <button class="toolsButton" @click="fontSize(true)">+</button>
    </div>
    <div class="meta">{{ songData.name }} - {{ songData.artist }}</div>
  </div>
</template>

<script>

export default {
  props: {
    songData: Array,
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
  background: #010223;
  overflow-x: scroll;
  color: white;
  &::-webkit-scrollbar {
    display: none;
  }
}
.close {
  position: fixed;
  bottom: 10px;
  left: 10px;
  width: 50px;
  height: 50px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border-radius: 10px;
  cursor: pointer;
  z-index: 99;
  font-weight: bold;
  border: none;
  &::before {
    content: '<';
  }
}
.tools {
  position: fixed;
  right: 10px;
  bottom: 10px;
  z-index: 99;
  cursor: pointer;
  &Button {
    background: rgba(255, 255, 255, 0.2);
    color: white;
    border: none;
    border-radius: 10px;
    font-size: 20px;
    width: 50px;
    height: 50px;
    margin-right: 10px;
  }
}
.meta {
  position: fixed;
  bottom: 25px;
  left: 50%;
  transform: translateX(-50%);
  max-width: 50%;
  color: white;
  opacity: 0.5;
}
.lyrics {
  position: relative;
  top: 30px;
  height: calc(100% - 100px);
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
    position: relative;
    overflow: hidden;
    height: 4em;
    margin-bottom: 0.8em;
    &::before {
      content: '🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸🎸';
      font-size: 2em;
    }
    &::after {
      content: '';
      display: block;
      width: 80px;
      height: 100%;
      background: #010223;
      top: 0;
      right: 0;
      position: absolute;
    }
  }
  &[data-type='refrain'] {
    color: yellow;
  }
  &[data-type='italic'] {
    font-style: italic;
  }
}
</style>
