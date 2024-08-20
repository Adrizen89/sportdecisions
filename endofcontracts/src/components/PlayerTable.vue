<template>
  <div class="player-table">
    <table class="table table-hover w-100">
      <!-- Utilisation du composant HeadingTable avec les colonnes dynamiques -->
      <HeadingTable
        :columns="isMobile ? [localColumns[currentColumn]] : localColumns"
        :currentSortColumn="currentSortColumn"
        :currentSortDirection="currentSortDirection"
        @sort-column="setSortColumn"
        @move-first-column="moveFirstColumnToEnd"
        @move-last-column="moveLastColumnToStart"
      />
      <tbody>
        <tr v-for="player in sortedPlayers" :key="player.id">
          <td>
            <PlayerInfo :player="player" />
          </td>
          <td
            v-for="(column, index) in localColumns"
            :key="column.key"
            :class="[
              getStatusClass(column.key, player.contract_end_status),
              { 'd-none d-md-table-cell': isMobile && index !== currentColumn },
              { 'd-table-cell': !isMobile || index === currentColumn }
            ]"
          >
            {{ player[column.key] ? formatDate(player[column.key]) : '--' }}
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
      localColumns: [
        { label: 'End of Contracts', key: 'contract_end' },
        { label: 'Option', key: 'option' },
        { label: 'Option-validity', key: 'option_validity' }
      ],
      currentSortColumn: null,
      currentSortDirection: 'asc',
      currentColumn: 0, // Colonne actuellement affichée en mode mobile
      isMobile: window.innerWidth < 768 // Détecter si l'écran est mobile
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
      if (!dateStr) return ''
      const date = new Date(dateStr)
      const day = String(date.getDate()).padStart(2, '0')
      const month = String(date.getMonth() + 1).padStart(2, '0')
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
    setSortColumn(columnKey) {
      if (this.currentSortColumn === columnKey) {
        this.currentSortDirection = this.currentSortDirection === 'asc' ? 'desc' : 'asc'
      } else {
        this.currentSortColumn = columnKey
        this.currentSortDirection = 'asc'
      }
    },
    moveFirstColumnToEnd() {
      const firstColumn = this.localColumns.shift()
      this.localColumns.push(firstColumn)
    },
    moveLastColumnToStart() {
      const lastColumn = this.localColumns.pop()
      this.localColumns.unshift(lastColumn)
    },
    // Méthodes pour naviguer entre les colonnes en mode mobile
    previousColumn() {
      if (this.currentColumn === 0) {
        this.currentColumn = this.localColumns.length - 1
      } else {
        this.currentColumn--
      }
    },
    nextColumn() {
      if (this.currentColumn === this.localColumns.length - 1) {
        this.currentColumn = 0
      } else {
        this.currentColumn++
      }
    },
    handleResize() {
      this.isMobile = window.innerWidth < 768
    }
  },
  mounted() {
    window.addEventListener('resize', this.handleResize)
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize)
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
  vertical-align: middle;
}
td:first-child {
  width: 33%;
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
