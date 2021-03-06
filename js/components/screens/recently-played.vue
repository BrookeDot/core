<template>
  <section id="recentlyPlayedWrapper">
    <screen-header>
      Recently Played
      <controls-toggler :showing-controls="showingControls" @toggleControls="toggleControls"/>

      <template v-slot:meta>
        <span v-if="meta.songCount">{{ meta.songCount | pluralize('song') }} • {{ meta.totalLength }}</span>
      </template>

      <template v-slot:controls>
        <song-list-controls
          v-if="state.songs.length && (!isPhone || showingControls)"
          @playAll="playAll"
          @playSelected="playSelected"
          :songs="state.songs"
          :config="songListControlConfig"
          :selectedSongs="selectedSongs"
        />
      </template>
    </screen-header>

    <song-list v-if="state.songs.length" :items="state.songs" type="recently-played" :sortable="false"/>

    <div v-if="!state.songs.length" class="none text-light-gray">
      This playlist is automatically populated with the songs you most recently played, so start playing!
    </div>
  </section>
</template>

<script lang="ts">
import { eventBus, pluralize } from '@/utils'
import { events } from '@/config'
import { recentlyPlayedStore } from '@/stores'
import hasSongList from '@/mixins/has-song-list.ts'
import mixins from 'vue-typed-mixins'

export default mixins(hasSongList).extend({
  components: {
    ScreenHeader: () => import('@/components/ui/screen-header.vue')
  },

  filters: { pluralize },

  data: () => ({
    state: recentlyPlayedStore.state
  }),

  methods: {
    getSongsToPlay (): Song[] {
      return this.state.songs
    }
  },

  created (): void {
    eventBus.on({
      [events.LOAD_MAIN_CONTENT]: (view: MainViewName): void => {
        if (view === 'RecentlyPlayed') {
          recentlyPlayedStore.fetchAll()
        }
      }
    })
  }
})
</script>

<style lang="scss">
@import "~#/partials/_vars.scss";

#recentlyPlayedWrapper {
  .none {
    padding: 16px 24px;
  }
}
</style>
