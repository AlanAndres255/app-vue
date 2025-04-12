<script setup>
import { ref } from 'vue';
import axios from 'axios';

// Variables reactivas
const selectedTable = ref('universes'); // Tabla seleccionada
const tableData = ref([]); // Datos de la tabla seleccionada
const recordId = ref(''); // ID del registro a buscar
const recordData = ref(null); // Datos del registro específico

// URLs de los endpoints
const endpoints = {
  universes: 'http://127.0.0.1:8000/api/universes',
  genders: 'http://127.0.0.1:8000/api/genders',
  superhero: 'http://127.0.0.1:8000/api/superhero',
};

// Función para obtener todos los registros de la tabla seleccionada
async function fetchTableData() {
  const response = await axios.get(endpoints[selectedTable.value]);
  tableData.value = response.data;
}

// Función para obtener un registro específico por ID
async function fetchRecordData() {
  const response = await axios.get(`${endpoints[selectedTable.value]}/${recordId.value}`);
  recordData.value = response.data;
}
</script>

<template>
  <div style="display: flex; gap: 20px; padding: 20px;">
    <!-- Selector de tabla y salida de nombres -->
    <div style="flex: 1; border-right: 1px solid #ccc; padding-right: 20px;">
      <h2>Choose a Table</h2>
      <div style="margin-bottom: 20px;">
        <label>Choose a table:</label>
        <select v-model="selectedTable" @change="fetchTableData">
          <option value="universes">Universes</option>
          <option value="genders">Genders</option>
          <option value="superhero">Superheroes</option>
        </select>
        <button @click="fetchTableData">List Records</button>
      </div>

      <ul>
        <li v-for="record in tableData" :key="record.id">
          {{ record.name || record.title || record.id }}
        </li>
      </ul>
    </div>

    <!-- Buscar por ID y lista completa -->
    <div style="flex: 1; padding-left: 20px;">
      <h2>Find Record</h2>
      <div style="margin-bottom: 20px;">
        <label>Enter ID:</label>
        <input type="text" v-model="recordId" />
        <button @click="fetchRecordData">Find Record</button>
      </div>

      <div v-if="recordData">
        <h3>Record Details:</h3>
        <pre>{{ recordData }}</pre>
      </div>
    </div>
  </div>
</template>