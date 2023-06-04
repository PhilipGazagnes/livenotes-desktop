<template>
  <SongDetail
    v-if="songData"
    :song-data="songData"
    @close-song="handleCloseSong"
  />
  <SongList
    :song-list="$options.listData"
    @song-select="handleSongSelect"
    @wait="handleWait(true)"
  />
  <WaitScreen
    v-if="waitScreen"
    @click="handleWait(false)"
  />
</template>

<script>
import songsDataJson from './data/data.json';
import listDataJson from './data/index.json';

import SongList from './components/SongList.vue';
import SongDetail from './components/SongDetail.vue';
import WaitScreen from './components/WaitScreen.vue';

export default {
  songsData: songsDataJson,
  // listData: listDataJson.filter(s => s.scope.includes('funlive')).sort(compare), 
  listData: listDataJson.sort(compare), 
  components: {
    SongList,
    SongDetail,
    WaitScreen,
  },
  data() {
    return {
      songData: null,
      waitScreen: true,
    };
  },
  methods: {
    freezeBody() {
      document.body.style.overflow = 'hidden';
    },
    unfreezeBody() {
      document.body.style.overflow = 'auto';
    },
    flattenLyrics(sections) {
      return sections.reduce((acc, section) => {
        
        if (section.lyrics) {
          section.lyrics.forEach(lyric => {
            if (lyric.includes('***')) {
              if (acc.length && acc[acc.length - 1].type !== 'instru') {
                acc.push({
                  type: 'instru',
                  lyric: '',
                });
              }
            } else if (lyric.includes('+++')) {
              acc.push({
                type: 'italic',
                lyric: lyric.replaceAll('+', ''),
              });
            } else {
              acc.push({
                type: section.name.split('!')[0].toLowerCase(),
                lyric,
              });
            }
          });
          acc.push({
            type: 'separator',
            lyric: '',
          });
        }
        return acc;
      }, []);
    },
    handleSongSelect(payload) { // [id, name, artist]
      this.freezeBody();
      this.songData = {
        lyrics: this.flattenLyrics(this.$options.songsData[payload[0]].sections),
        name: payload[1],
        artist: payload[2],
      };
    },
    handleCloseSong() {
      this.songData = null;
      this.unfreezeBody();
    },
    handleWait(param) {
      this.waitScreen = param;
      document.body.style.overflow = param ? 'hidden' : 'auto';
    },
  },
};

function compare(a, b) {
  function regularFirstLetter(str) {
    let txt = str;
    if (txt.startsWith('ç') || txt.startsWith('Ç')) {
      txt = `C${txt.substring(1)}`;
    }
    return txt;
  }
  // Use toUpperCase() to ignore character casing
  const songA = regularFirstLetter(a.name.toUpperCase());
  const songB = regularFirstLetter(b.name.toUpperCase());
  let comparison = 0;
  if (songA > songB) {
    comparison = 1;
  } else if (songA < songB) {
    comparison = -1;
  }
  return comparison;
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  background: #010223;
}
</style>