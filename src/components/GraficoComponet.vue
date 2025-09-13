<template>
  <div>
    <!-- Grafico -->
    <apexchart 
      type="line"
      height="400"
      width="800"
      :options="chartOptions"
      :series="series"
    />
  </div>
</template>

<script>
import { computed, defineComponent } from "vue"

export default defineComponent({
  name: "GraficoComponet",
  props: {
    categories: { type: Array, default: () => [] },
    series: { type: Array, default: () => [] }
  },
  setup(props) {
    const chartOptions = computed(() => ({
      chart: { id: "dados-diarios-arrecadacao", toolbar: { show: true } },
      xaxis: { categories: props.categories ?? [] },
      yaxis: {
        labels: { formatter: v => (v == null ? v : Number(v).toLocaleString("pt-BR")) }
      },
      tooltip: {
        y: { formatter: v => (v == null ? v : Number(v).toLocaleString("pt-BR")) }
      }
    }))

    return { chartOptions }
  }
})
</script>