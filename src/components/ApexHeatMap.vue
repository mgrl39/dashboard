<template>
  <div class="chart-container">
    <VueApexCharts
      type="heatmap"
      height="100%"
      :options="chartOptions"
      :series="series"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import VueApexCharts from "vue3-apexcharts";

// Datos que representan los votos únicos diarios por franja horaria
const series = ref([
  {
    name: "Lunes",
    data: [85, 145, 178, 123, 189, 56, 24],
  },
  {
    name: "Martes",
    data: [76, 158, 167, 134, 176, 45, 23],
  },
  {
    name: "Miércoles",
    data: [67, 163, 189, 145, 171, 67, 25],
  },
  {
    name: "Jueves",
    data: [84, 149, 156, 132, 168, 54, 22],
  },
  {
    name: "Viernes",
    data: [73, 147, 165, 129, 156, 42, 18],
  },
  {
    name: "Sábado",
    data: [15, 27, 19, 18, 16, 15, 14],
  },
  {
    name: "Domingo",
    data: [14, 26, 18, 17, 15, 14, 13],
  },
]);

const chartOptions = ref({
  chart: {
    type: "heatmap",
    height: "100%",
    toolbar: {
      show: false,
    },
    background: "transparent",
  },
  dataLabels: {
    enabled: false,
  },
  theme: {
    mode: "dark",
  },
  title: {
    text: "Distribución de Votos Diarios",
    align: "left",
    style: {
      fontSize: "16px",
      fontWeight: "600",
      color: "#f4f4f4",
    },
  },
  plotOptions: {
    heatmap: {
      shadeIntensity: 0.5,
      radius: 0,
      useFillColorAsStroke: true,
      colorScale: {
        ranges: [
          {
            from: 0,
            to: 50,
            name: "Baja",
            color: "#97092c",
          },
          {
            from: 51,
            to: 100,
            name: "Media",
            color: "#640e22",
          },
          {
            from: 101,
            to: 150,
            name: "Alta",
            color: "#561524",
          },
          {
            from: 151,
            to: 200,
            name: "Muy Alta",
            color: "#38101a",
          },
        ],
      },
    },
  },
  xaxis: {
    categories: ["8:30", "10:30", "12:30", "14:30", "15:30", "17:00", "18:00"],
    labels: {
      style: {
        colors: "#f4f4f4",
      },
    },
  },
  yaxis: {
    labels: {
      style: {
        colors: "#f4f4f4",
      },
    },
  },
  tooltip: {
    theme: "dark",
    y: {
      title: {
        formatter: function (value: string): string {
          return "Votos: ";
        },
      },
    },
  },
});
</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 100%;
  padding: 1rem;
}

.chart-title {
  color: #f4f4f4;
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 1rem;
}
</style>
