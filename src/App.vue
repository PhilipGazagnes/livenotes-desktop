<template>
  <SongDetail
    v-if="songData"
    :song-data="songData"
    @close-song="handleCloseSong"
  />
  <SongList
    :song-list="$options.listData"
    @song-select="handleSongSelect"
  />
</template>

<script>
import songsDataJson from './data/data.json';
import listDataJson from './data/index.json';
import SongList from './components/SongList.vue';
import SongDetail from './components/SongDetail.vue';

export default {
  songsData: songsDataJson,
  listData: listDataJson.sort(compare), 
  components: {
    SongList,
    SongDetail,
  },
  data() {
    return {
      songData: null,
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
          })
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