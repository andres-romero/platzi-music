<template lang="pug">
  #app
    pm-header
    pm-loader(v-show="isLoading")
    section.section(v-show="!isLoading")
      nav.nav
        .field.has-addons
          .control.is-expanded
            input.input(
              type="text"
              placeholder="Buscar canciones"
              v-model="searchQuery")
          .control
            button.button.is-info(@click="search") Buscar
          .control
            button.button.is-danger &times;
          .control
            button.button
              span.is-size-7 Encontrado: {{ searchMessage }}

      .container.results
        .columns.is-multiline
          .column.is-one-quarter(v-for="t in tracks") 
            pm-track(:track = "t")
    pm-footer
</template>

<script>
import trackService from '@/services/track'
import PmFooter from '@/components/layout/footer.vue'
import PmHeader from '@/components/layout/header.vue'

import PmTrack from '@/components/Track.vue'
import PmLoader from '@/components/shared/Loader.vue'

export default {
  name: 'app',

  components: { PmFooter, PmHeader, PmTrack, PmLoader },

  data () {
    return {
      tracks: [],
      searchQuery: '',

      isLoading: false
    }
  },
  methods: {
    search () {
      if (!this.searchQuery) { return }

      this.isLoading = true

      trackService.search(this.searchQuery)
        .then(res => {
          this.tracks = res.tracks.items
          this.isLoading = false
        })
    }
  },
  computed: {
    searchMessage () {
      return this.tracks.length
    }
  }
}
</script>

<style lang="scss">
@import 'scss/main.scss';
#app {
  .results {
    margin-top: 30px;
  }
}
</style>