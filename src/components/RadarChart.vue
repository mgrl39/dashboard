<template>
  <div class="chart-container">
    <v-chart class="chart" :option="option" autoresize />
  </div>
</template>

<script setup lang="ts">
import { ref, shallowRef, onMounted, onUnmounted } from "vue";
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { RadarChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
} from "echarts/components";
import VChart from "vue-echarts";
import echartsData from "../data/echarts-data.json";

// Register ECharts components
use([
  CanvasRenderer,
  RadarChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent,
]);

const radarData = ref(JSON.parse(JSON.stringify(echartsData.radar)));

const option = shallowRef({
  backgroundColor: "transparent",
  title: {
    text: "Radar de Géneros Musicales",
    left: "center",
    textStyle: {
      color: "#f4f4f4",
      fontSize: 14,
      fontWeight: "normal",
    },
  },
  tooltip: {
    trigger: "item",
  },
  legend: {
    data: ["Géneros Más Votados", "Géneros Más Escuchados"],
    bottom: 0,
    textStyle: {
      color: "#f4f4f4",
    },
  },
  radar: {
    indicator: radarData.value.indicator,
    radius: "65%",
    center: ["50%", "50%"],
    splitArea: {
      areaStyle: {
        color: ["#38101a", "#561524", "#640e22", "#97092c"],
      },
    },
    axisLine: {
      lineStyle: {
        color: "#f4f4f4",
      },
    },
    splitLine: {
      lineStyle: {
        color: "#f4f4f4",
      },
    },
    name: {
      textStyle: {
        color: "#f4f4f4",
      },
    },
  },
  series: radarData.value.series,
});

let intervalId: number | null = null;

const updateChartData = () => {
  const newData = JSON.parse(JSON.stringify(radarData.value));

  newData.series[0].data.forEach((app: any) => {
    app.value = app.value.map((value: number) => {
      const change =
        Math.floor(Math.random() * 5) * (Math.random() > 0.5 ? 1 : -1);
      return Math.min(100, Math.max(0, value + change));
    });
  });

  radarData.value = newData;

  option.value = {
    ...option.value,
    radar: {
      ...option.value.radar,
      indicator: radarData.value.indicator,
    },
    series: radarData.value.series,
  };
};

onMounted(() => {
  intervalId = window.setInterval(updateChartData, 2000);
});

onUnmounted(() => {
  if (intervalId !== null) {
    clearInterval(intervalId);
  }
});
</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 100%;
}

.chart {
  width: 100%;
  height: 100%;
}
</style>
