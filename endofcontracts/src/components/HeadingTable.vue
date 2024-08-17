<template>
  <thead class="thead-rounded">
    <tr class="header-row">
      <!-- Première cellule vide pour l'alignement avec PlayerInfo -->
      <th></th>

      <!-- Cellule pour contenir la div avec les icônes et les en-têtes -->
      <th colspan="3" class="no-padding">
        <div class="test">
          <i class="bi bi-caret-left-fill" @click="moveFirstColumnToEnd"></i>

          <div
            v-for="(column, index) in columns"
            :key="index"
            class="header-cell"
            :class="{ 'blue-background': isLastThreeColumns(index) }"
          >
            {{ column }}
          </div>

          <i class="bi bi-caret-right-fill" @click="moveLastColumnToStart"></i>
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
    columns: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      localColumns: [...this.columns] // Créer une copie locale des colonnes pour manipulation
    }
  },
  methods: {
    isLastThreeColumns(index) {
      // Retourne true pour les trois dernières colonnes
      return index >= this.localColumns.length - 3
    },

    moveFirstColumnToEnd() {
      const firstColumn = this.localColumns.shift() // Supprime la première colonne
      this.localColumns.push(firstColumn) // Ajoute la première colonne à la fin
    },

    moveLastColumnToStart() {
      const lastColumn = this.localColumns.pop() // Supprime la dernière colonne
      this.localColumns.unshift(lastColumn) // Ajoute la dernière colonne au début
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
  font-size: clamp(0.5rem, 1.5vw, 1rem);
  text-transform: uppercase;
  border-radius: 5px;
  width: 100%;
  margin: 0 1%;
  height: 6vh;
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
    font-size: clamp(0.4rem, 1.2vw, 0.8rem);
    padding: 8px;
  }
}
</style>
