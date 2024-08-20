<template>
  <thead class="thead-rounded">
    <tr class="header-row">
      <th></th>
      <!-- Première cellule vide pour l'alignement avec PlayerInfo -->
      <th colspan="3" class="no-padding">
        <div class="test">
          <i class="bi bi-caret-left-fill" @click="$emit('move-first-column')"></i>

          <div
            v-for="(column, index) in columns"
            :key="column.key"
            class="header-cell"
            @click="sortColumn(column.key)"
            :class="{
              'blue-background': isLastThreeColumns(index),
              sortable: true,
              'sorted-asc': currentSortColumn === column.key && currentSortDirection === 'asc',
              'sorted-desc': currentSortColumn === column.key && currentSortDirection === 'desc'
            }"
          >
            {{ column.label }}
          </div>

          <i class="bi bi-caret-right-fill" @click="$emit('move-last-column')"></i>
        </div>
      </th>
    </tr>
  </thead>
</template>

<script>
import 'bootstrap-icons/font/bootstrap-icons.css'

export default {
  name: 'HeadingTable',
  props: {
    columns: Array,
    currentSortColumn: String,
    currentSortDirection: String
  },
  methods: {
    isLastThreeColumns(index) {
      // Retourne true pour les trois dernières colonnes
      return index >= this.columns.length - 3
    },
    sortColumn(columnKey) {
      this.$emit('sort-column', columnKey)
    },
    moveFirstColumnToEnd() {
      this.$emit('move-first-column')
    },
    moveLastColumnToStart() {
      this.$emit('move-last-column')
    }
  }
}
</script>

<style scoped>
i {
  color: #8899a2;
  margin: 5px;
  cursor: pointer; /* Ajoute un curseur pointeur pour indiquer que c'est cliquable */
}
i:hover {
  color: #d6d6d6;
}
.test {
  width: 100%;
  background-color: #354751;
  border-radius: 5px;
  height: 7vh;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

th:first-child {
  background-color: #000;
  border-radius: none;
  border: none;
}

.header-cell {
  background-color: #000;
  color: #66acd6;
  text-align: center;
  font-size: clamp(0.7rem, 1.5vw, 1rem);
  font-weight: bold;
  text-transform: uppercase;
  border-radius: 5px;
  width: 100%;
  margin: 0 1%;
  height: 6vh;
  cursor: pointer;
}
.header-cell:hover {
  background-color: #181818;
}

.no-padding {
  padding: 0;
  background-color: #000;
  border: none;
}

.header-row {
  background-color: #354751;
}

.header-row th:first-child {
  border-radius: 10px 0 0 10px;
}

.header-row th:last-child {
  border-radius: 0 10px 10px 0;
}

/* Responsiveness */
@media screen and (max-width: 768px) {
  .blue-background::before {
    transform: translateY(-5px) scaleY(1.2); /* Ajuster pour petits écrans */
  }

  .header-cell {
    padding: 8px;
  }
}
</style>
