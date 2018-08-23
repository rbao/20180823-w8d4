<template>
  <li>
    <span>{{url.shortUrl}}</span> - <a :href="url.longUrl" target="_blank">{{url.longUrl}}</a>
    <button @click="editClicked">Edit</button>
  </li>
</template>

<script>
export default {
  name: 'UrlItem',
  data () {
    return {}
  },
  props: {
    url: {
      required: true,
      type: Object
    }
  },
  methods: {
    editClicked () {
      this.$emit('edit', this.url.id)
    }
  }
}
</script>

<style>
</style>
