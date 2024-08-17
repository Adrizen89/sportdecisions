<template>
  <div class="player-table">
    <table class="table table-hover w-100">
      <!-- Utilisation du composant HeadingTable avec les colonnes dynamiques -->
      <HeadingTable :columns="localColumns" @update-columns="updateColumns" />
      <tbody>
        <tr v-for="player in sortedPlayers" :key="player.id">
          <td>
            <PlayerInfo :player="player" />
            <!-- Utilisation du composant PlayerInfo -->
          </td>
          <td
            v-for="(column, index) in localColumns"
            :key="index"
            :class="getStatusClass(column, player.contract_end_status)"
          >
            {{ formatDate(player[column]) }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { players } from '../data/players.js'
import PlayerInfo from './PlayerInfo.vue'
import HeadingTable from './HeadingTable.vue'

export default {
  name: 'EndOfContracts',
  components: {
    PlayerInfo,
    HeadingTable
  },

  data() {
    return {
      players,
      columnNames: ['contract_end', 'option', 'option_validity'], // Ajout de 'Player Info'
      localColumns: ['contract_end', 'option', 'option_validity'], // Colonnes initiales
      currentSortColumn: null,
      currentSortDirection: 'asc'
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
  },

  methods: {
    formatDate(dateStr) {
      if (!dateStr) return '' // Gérer les cas où la date est nulle
      const date = new Date(dateStr)
      const day = String(date.getDate()).padStart(2, '0')
      const month = String(date.getMonth() + 1).padStart(2, '0') // Les mois commencent à 0
      const year = date.getFullYear()
      return `${day}-${month}-${year}`
    },

    getStatusClass(columnKey, status) {
      if (columnKey === 'contract_end') {
        return {
          'status-green': status === 'green',
          'status-yellow': status === 'yellow',
          'status-red': status === 'red'
        }
      }
      return ''
    },
    updateColumns(newColumns) {
      this.localColumns = newColumns
    }
  }
}
</script>

<style scoped>
.player-table {
  padding: 3vh;
}
td {
  width: 20%;
  background-color: #000;
  border-bottom: 2px dotted #fff;
  color: #fff;
}
td:not(:first-child) {
  text-align: center;
}

.status-green {
  color: #28a745;
}

.status-yellow {
  color: #ffc107;
}

.status-red {
  color: #dc3545;
}
</style>
