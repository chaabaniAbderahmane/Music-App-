<template>
  <div class="fixed bottom-0 left-0 bg-white px-4 py-2 w-full">
    <!-- Track Info -->
    <div class="text-center" v-if="current_song.modified_name">
      <span class="song-title font-bold">{{current_song.modified_name}}</span> {{$t("Player.by")}}
      <span class="song-artist">{{current_song.display_name}}</span>
    </div>
    <div class="flex flex-nowrap gap-4 items-center">
      <!-- Play/Pause Button -->
      <button 
      id="player-play-btn"
      type="button" @click.prevent="toggleaudio"  >
        <i
          class="fa text-gray-500 text-xl"
          :class="{ 'fa-play': !playing, 'fa-pause': playing }"
        ></i>
      </button>
      <!-- Current Position -->
      <div class="player-currenttime">{{ seek }}</div>
      <!-- Scrub Container  -->
      <div @click.prevent="updateSeek" class="w-full h-2 rounded bg-gray-200 relative cursor-pointer">
        <!-- Player Ball -->
        <span
          class="absolute -top-2.5 -ml-2.5 text-gray-800 text-lg"
          :style="{ left: playerprogress }"
        >
          <i class="fas fa-circle"></i>
        </span>
        <!-- Player Progress Bar-->
        <span
          class="block h-2 rounded bg-gradient-to-r from-green-500 to-green-400"
          :style="{ width: playerprogress }"
        ></span>
      </div>
      <!-- Duration -->
      <div class="player-duration">{{ duration }}</div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from 'pinia'

import usePlayerStore from '@/stores/Player'

export default {
  name: 'AppPlayer',

  methods: {
    ...mapActions(usePlayerStore, ['toggleaudio', 'updateSeek'])
  },
  computed: {
    ...mapState(usePlayerStore, ['playing', 'seek', 'duration', 'playerprogress','current_song'])
  }
}
</script>
