<template>
  <div class="chart-container">
    <Pie :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  CategoryScale,
  type ChartData,
  type ChartOptions,
} from "chart.js";
import { Pie } from "vue-chartjs";

// Register Chart.js components
ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

const chartData = computed<ChartData<"pie">>(() => {
  return {
    labels: ["ESO", "Bachillerato", "Ciclos Formativos"],
    datasets: [
      {
        data: [60, 25, 15],
        backgroundColor: ["#97092c", "#640e22", "#561524"],
        borderColor: "#38101a",
        borderWidth: 1,
      },
    ],
  };
});

const chartOptions: ChartOptions<"pie"> = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: "bottom" as const,
      labels: {
        color: "#f4f4f4",
        boxWidth: 12,
        font: {
          size: 12,
        },
      },
    },
    title: {
      display: true,
      text: "Distribución de Estudiantes",
      color: "#f4f4f4",
      font: {
        size: 14,
      },
    },
    tooltip: {
      callbacks: {
        label: function (context: any) {
          const label = context.label || "";
          const value = context.raw || 0;
          const total = context.chart.data.datasets[0].data.reduce(
            (a: number, b: number) => a + b,
            0
          );
          const percentage = Math.round((value / total) * 100);
          return `${label}: ${percentage}%`;
        },
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
  padding: 1rem;
}
</style>
