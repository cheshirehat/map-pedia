<template>
  <div>
    <div v-if="searchTitle">
      <h2 class="sentence-title title is-4">{{ searchTitle }}</h2>
    </div>
    <div></div>
    <div v-html="sentence" class="sentence-contents">
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import EventBus from '../service/EventBus.js'

export default {
  data () {
    return {
      text: '',
      searchTitle: '',
      sentence: ''
    }
  },
  mounted () {

  },
  created () {
    EventBus.$on('search-wiki', this.searchWiki)
  },
  methods: {
    searchWiki (key) {
      const keyword = encodeURIComponent(key)
      const url = `https://ja.wikipedia.org/w/api.php?action=query&format=json&prop=extracts&redirects=1&rvprop=content&formatversion=2&titles=${keyword}&origin=*`
      axios({
        method: 'GET',
        url: url,
        responseType: 'json'
      })
      .then(response => {
        if (!response.status == 200) return this.sentence = '200じゃないからおけまるじゃない'
        if (response.data.query.pages[0].extract == undefined) {
          this.searchTitle = ''
          this.sentence = '<p class="search-out">検索できなかったやつ(´・ω・`)</p>'
          return
        }
        const data = response.data.query.pages[0].extract
        this.searchTitle = key + 'の検索結果'
        this.sentence = data
      })
      .catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<style>
.sentence-title {
  margin-top: 20px;
}
.sentence-contents {
  height: 500px;
  margin-top: 5px;
  margin-right: 40px;
  overflow: scroll;
}
.search-out {
  position: relative;
  top: 50%;
  text-align: center;
  font-size: 2em;
}
</style>

