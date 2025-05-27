<template>
  <div class="chart-container">
    <Doughnut :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { Doughnut } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  CategoryScale,
} from "chart.js";

// Register Chart.js components
ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

const chartData = computed(() => ({
  labels: ["Errores", "Éxitos", "Pendientes"],
  datasets: [
    {
      data: [15, 75, 10],
      backgroundColor: [
        "#ef4444", // rojo para errores
        "#22c55e", // verde para éxitos
        "#f59e0b", // amarillo para pendientes
      ],
      borderWidth: 0,
    },
  ],
}));

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  cutout: "65%",
  plugins: {
    legend: {
      position: "bottom" as const,
      labels: {
        color: "#f4f4f4",
        boxWidth: 12,
      },
    },
    title: {
      display: true,
      text: "Estado de Peticiones",
      color: "#f4f4f4",
      font: {
        size: 14,
      },
    },
  },
};
</script>

<style scoped>
.chart-container {
  position: relative;
  width: 100%;
  height: 100%;
  padding: 10px;
}
</style>
