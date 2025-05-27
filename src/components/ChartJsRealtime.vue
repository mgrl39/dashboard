<template>
  <div class="chart-container">
    <Line :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  type ChartData,
  type ChartOptions,
} from "chart.js";
import { Line } from "vue-chartjs";

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

const initialData: ChartData<"line"> = {
  labels: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
  datasets: [
    {
      label: "Votos en Tiempo Real",
      data: [40, 45, 42, 47, 45, 48, 43, 49, 45, 47],
      borderColor: "#97092c",
      backgroundColor: "#640e22",
      fill: true,
      tension: 0.4,
    },
  ],
};

const chartData = ref<ChartData<"line">>(initialData);

const chartOptions: ChartOptions<"line"> = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      position: "top" as const,
      labels: {
        color: "#f4f4f4",
      },
    },
    title: {
      display: true,
      text: "Votos por Segundo",
      color: "#f4f4f4",
      font: {
        size: 16,
      },
    },
  },
  scales: {
    y: {
      beginAtZero: true,
      grid: {
        color: "#561524",
      },
      ticks: {
        color: "#f4f4f4",
      },
    },
    x: {
      grid: {
        color: "#561524",
      },
      ticks: {
        color: "#f4f4f4",
      },
    },
  },
};

let intervalId: number | null = null;

const updateData = () => {
  const newData = [...chartData.value.datasets[0].data];
  newData.shift();
  newData.push(Math.floor(Math.random() * 20) + 35);

  chartData.value = {
    ...chartData.value,
    datasets: [
      {
        ...chartData.value.datasets[0],
        data: newData,
      },
    ],
  };
};

onMounted(() => {
  intervalId = window.setInterval(updateData, 1000);
});

onUnmounted(() => {
  if (intervalId !== null) {
    clearInterval(intervalId);
  }
});
</script>

<style scoped>
.chart-container {
  position: relative;
  width: 100%;
  height: 100%;
  padding: 1rem;
}
</style>
