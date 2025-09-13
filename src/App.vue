<template>
  <div class="app-container">
    <div class="card">

      <!-- Titulo--> 
      
        <h2 class="card-title"> Dados diarios de Arrecadação</h2>
      
      <!-- Filtro --> 
      <div  class="filters">

        <div class="filter-item">

          <label for="dateInicial" > Data Inicial</label>
          <input 
            id="dateInitial"
            type="date"
            v-model="dateInitial"
            class="border px-2 py-1 rounded"
          
          />
          
        </div>

        <div class="filter-item">

          <label for="dateFinal" > Data Final</label>
          <input 
            id="dateFinal"
            type="date"
            v-model="dateFinal"
            class="border px-2 py-1 rounded"  
          
          />
        
        </div>

        <button 
          @click="filtrarData" class="btn-update">
            Atualizar
        </button>

      </div>

        <!-- Componente -->  
      <GraficoComponet  :categories="categories" :series="series"/>  
  
      
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"
import axios from "axios"
import GraficoComponet from "./components/GraficoComponet.vue"

const dateInitial = ref("2025-09-08")
const dateFinal = ref("2025-09-08")
const categories = ref([])
const series = ref([])

async function filtrarData() {
  try {
    const response = await axios.post(
      "https://polvo-api.consensotec.com.br/polvo-chart-rest-api/chart",
      {
        chartID: "dadosDiariosArrecadacao",
        accountID: 16,
        userID: 24,
        payload: {
          dateInitial: dateInitial.value,
          dateFinal: dateFinal.value,
          companyIsMunicipal: false,
          isContabil: false,
        },
      },
      {
        headers: { "Content-Type": "application/json" }
      }
    )

    const data = response.data

    categories.value = data?.categories ?? []
    series.value = data?.series ?? []
  } catch (err) {
    console.error("Erro ao buscar dados:", err)
  }
}

onMounted(() => {
  filtrarData()
})
</script>

<style >
  /* Container do App */
  .app-container {
    min-height: 100vh;
    background-color: #f3f4f6;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 2rem 1rem;
  }

  /* Card principal */
  .card {
    background-color: #ffffff;
    border-radius: 1.5rem;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    padding: 2rem;
    width: 100%;
    max-width: 900px;
  }

  /* Título */
  .card-title {
    text-align: center;
    font-size: 2rem;
    font-weight: bold;
    color: #1f2937;
    margin-bottom: 2rem;
  }

  /* Filtros */
  .filters {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 2rem;
    justify-content: center;
    align-items: flex-end;
  }

  .filter-item {
    display: flex;
    flex-direction: column;
    flex: 1 1 150px;
  }

  .filter-item label {
    font-weight: 500;
    margin-bottom: 0.25rem;
    color: #374151;
  }

  .filter-item input {
    padding: 0.5rem 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    outline: none;
    transition: all 0.2s;
  }

  .filter-item input:focus {
    border-color: #3b82f6;
    box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.2);
  }

  /* Botão */
  .btn-update {
    background-color: #3b82f6;
    color: white;
    font-weight: 600;
    padding: 0.5rem 1.5rem;
    border-radius: 0.5rem;
    border: none;
    cursor: pointer;
    transition: background-color 0.2s;
    align-self: flex-start;
  }

  .btn-update:hover {
    background-color: #2563eb;
  }

  /* Responsividade */
  @media (max-width: 640px) {
    .filters {
      flex-direction: column;
      align-items: stretch;
    }

    .btn-update {
      align-self: stretch;
    }
  }
</style>