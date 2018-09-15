<template>
  <div>
    <h1 class="title is-3 title-cont">検索！検索！</h1>
    <form class="level-item" v-on:submit.prevent="search()">
      <div class="field has-addons">
        <p class="control">
          <input class="input" v-model="text" type="text" placeholder="Find a post">
        </p>
        <p class="control">
          <button class="button">
            Search
          </button>
        </p>
      </div>
    </form>
  </div>
</template>

<script>
import EventBus from '../service/EventBus.js'

export default {
  data () {
    return {
      text: '',
    }
  },
  methods: {
    search () {
      EventBus.$emit('search-geocode', this.text)
      EventBus.$emit('search-wiki', this.text)
      const resetText = () => this.text = ''
      setTimeout(resetText, 500)
    }
  }
}
</script>

<style>
.title-cont {
  text-align: center;
  margin-top: 20px;
}
</style>
