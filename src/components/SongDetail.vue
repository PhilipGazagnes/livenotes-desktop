<template>
  <div
    class="song"
    id="song"
  >
    <button
      class="close"
      @click="handleClose"
    />
    <button
      class="toggleMode"
      @click="handleToggleMode"
    >=</button>
    <button
      v-if="displayMode === 'vertical'"
      class="autoscroll"
      @click="handleAutoScroll"
    >{{ autoScrollSpeed }}</button>
    
    <div
      class="lyrics"
      :data-display-mode="displayMode"
      :style="{ fontSize: `${fontSizeUser}em` }"
    >
      <div
        v-for="(lyric, index) in songData.lyrics"
        :key="index"
        class="lyric"
        :data-display-mode="displayMode"
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
      displayMode: 'vertical', // or 'horizontal'
      autoScrollSpeed: 0,
      autoScrollTimer: null,
    };
  },
  mounted() {
    console.log(this.songData.lyrics);
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
    handleToggleMode() {
      clearInterval(this.autoScrollTimer);
      this.autoScrollSpeed = 0;
      this.displayMode = this.displayMode === 'horizontal' ? 'vertical' : 'horizontal';
    },
    scrollDown() {
      document.getElementById('song').scrollTop = document.getElementById('song').scrollTop + 1;
    },
    handleAutoScroll() {
      clearInterval(this.autoScrollTimer);
      this.autoScrollSpeed = this.autoScrollSpeed < 3 ? this.autoScrollSpeed + 1 : 0;
      if(this.autoScrollSpeed) {
        this.autoScrollTimer = setInterval(() => {
          requestAnimationFrame(this.scrollDown);
        }, [50, 25, 15][this.autoScrollSpeed - 1]);
      } else {
        clearInterval(this.autoScrollTimer);
      }
    },
    handleClose() {
      clearInterval(this.autoScrollTimer);
      this.autoScrollSpeed = 0;
      this.$emit('close-song');
    }
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
  &::after {
    content: '';
    display: block;
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 120px;
    background: #010223;
    background: linear-gradient(0deg, rgba(1,2,35,1) 70%, rgba(1,2,35,0) 100%);
    z-index: 0;
  }
}
.close {
  position: fixed;
  bottom: 10px;
  left: 10px;
  width: 80px;
  height: 50px;
  background: rgba(255, 255, 255, 0.1);
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
.toggleMode {
  position: fixed;
  bottom: 10px;
  left: 100px;
  width: 80px;
  height: 50px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-radius: 10px;
  cursor: pointer;
  z-index: 99;
  font-weight: bold;
  border: none;
}
.autoscroll {
  position: fixed;
  bottom: 10px;
  left: 190px;
  width: 80px;
  height: 50px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-radius: 10px;
  cursor: pointer;
  z-index: 99;
  font-weight: bold;
  border: none;
}
.tools {
  position: fixed;
  right: 10px;
  bottom: 10px;
  z-index: 99;
  cursor: pointer;
  &Button {
    background: rgba(255, 255, 255, 0.1);
    color: white;
    border: none;
    border-radius: 10px;
    font-size: 20px;
    width: 80px;
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
  z-index: 99;
}
.lyrics {
  &[data-display-mode='horizontal'] {
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
  &[data-display-mode='vertical'] {
    text-align: center;
    padding: 300px 0 200px 0;
  }
}
.lyric {
  font-weight: bold;
  line-height: 1.5em;
  color: white;
  &[data-display-mode='horizontal'] {
    width: 500px;
    padding: 0 80px 0.8em 0;
    &[data-type='instru'] {
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
  }
  &[data-display-mode='vertical'] {
    padding: 10px 0;
    &[data-type='instru'] {
      position: relative;
      overflow: hidden;
      margin-bottom: 0.8em;
      margin-top: 0.8em;
      &::before {
        content: '🎸🎸🎸🎸🎸🎸🎸';
        margin-top: 0.8em;
        font-size: 2em;
      }
    }
    &[data-type='separator'] {
      padding: 0;
      height: 40px;
      position: relative;
      &::after {
        content: '';
        display: block;
        position: absolute;
        width: 200px;
        height: 2px;
        background: rgba(255, 255, 255, 0.2);
        left: 50%;
        top: 50%;
        transform: translateX(-50%) translateY(-50%);
      }
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
