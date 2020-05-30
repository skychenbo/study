<template>
  <div id="app">
     <v-header :seller="seller"></v-header>
  </div>
</template>

<script>
import qs from 'query-string'
import { getSeller } from 'api'
import VHeader from './components/v-header/v-header'

export default {
  name: 'App',
  data() {
    return {
      seller: {
        id: qs.parse(location.search).id
      }
    }
  },
  created() {
    this._getSeller()
  },
  methods: {
    _getSeller() {
      getSeller({
        id: this.seller.id
      }).then((seller) => {
        this.seller = Object.assign({}, this.seller, seller)
      })
    }
  },
  components: {
    VHeader
  }
}
</script>

<style lang="stylus">
</style>
