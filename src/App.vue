<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{title}}</h1>

    <ul>
      <url-item v-for="url in urlDatabase" :url="url" @edit="editUrl"></url-item>
    </ul>

    <form @submit.prevent="submit">
      <input v-model="urlObject.shortUrl" type="text" name="short-url" placeholder="Enter a short url...">
      <input v-model="urlObject.longUrl" type="text" name="long-url" placeholder="Enter a long url...">

      <button type="submit">Submit</button>
      <button v-if="editingId" @click="cancel" type="button">Cancel</button>
    </form>
  </div>
</template>

<script>
import UrlItem from './components/url-item'

export default {
  name: 'app',
  components: {
    UrlItem
  },
  data () {
    return {
      title: 'TinyApp',
      urlObject: {
        shortUrl: '',
        longUrl: ''
      },
      editingId: '',
      nextId: 3,
      urlDatabase: [
        { id: 1, shortUrl: 'gg', longUrl: 'http://google.ca' },
        { id: 2, shortUrl: 'fb', longUrl: 'http://facebook.com' }
      ]
    }
  },
  methods: {
    submit (e) {
      if (!this.urlObject.shortUrl || !this.urlObject.longUrl) {
        return
      }

      if (this.editingId) {
        // 1. Find url we are currently editing
        let targetUrlObject = this.urlDatabase.find((urlObject) => {
          return urlObject.id === this.editingId
        })

        // 2. Update properties accordingly
        targetUrlObject.shortUrl = this.urlObject.shortUrl
        targetUrlObject.longUrl = this.urlObject.longUrl

        this.cancel()
      } else {
        this.urlObject.id = this.nextId
        this.urlDatabase.push(this.urlObject)
        this.nextId = this.nextId + 1

        this.cancel()
      }
    },

    editUrl (urlId) {
      this.editingId = urlId

      // 1. Find the existing url object inside the database
      let targetUrlObject = this.urlDatabase.find((urlObject) => {
        return urlObject.id === urlId
      })

      // 2. Prefill the input with existing information
      this.urlObject = Object.assign({}, targetUrlObject)
    },

    cancel () {
      this.editingId = undefined

      this.urlObject = { shortUrl: '', longUrl: '' }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  margin-left: 10px;
}
</style>
