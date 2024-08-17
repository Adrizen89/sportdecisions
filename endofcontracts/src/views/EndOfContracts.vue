<template>
  <h1>End Of Contracts</h1>
  <div v-for="player in sortedPlayers" :key="player.id">
    <PlayerInfo :player="player"></PlayerInfo>
  </div>
</template>

<script>
import { players } from '../data/players.js'
import PlayerInfo from '../components/PlayerInfo.vue'

export default {
  name: 'EndOfContracts',
  components: {
    PlayerInfo
  },

  data() {
    return {
      players
    }
  },

  computed: {
    sortedPlayers() {
      if (!this.currentSortColumn) {
        return this.players
      }

      return this.players.slice().sort((a, b) => {
        let compareA = a[this.currentSortColumn]
        let compareB = b[this.currentSortColumn]

        // Pour le cas des dates, on peut comparer leur valeur numérique
        if (
          this.currentSortColumn.includes('contract_end') ||
          this.currentSortColumn.includes('option')
        ) {
          compareA = new Date(compareA).getTime()
          compareB = new Date(compareB).getTime()
        } else {
          compareA = compareA.toString().toLowerCase()
          compareB = compareB.toString().toLowerCase()
        }

        if (this.currentSortDirection === 'asc') {
          return compareA > compareB ? 1 : -1
        } else {
          return compareA < compareB ? 1 : -1
        }
      })
    }
  }
}
</script>
