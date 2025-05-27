<template>
  <VEChart class="gauge-ring-chart" :option="chartOptions" autoresize />
</template>

<script setup lang="ts">
import { computed } from "vue";
import { use } from "echarts/core";
import { GaugeChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
import { TooltipComponent, TitleComponent } from "echarts/components";
import VEChart from "vue-echarts";

use([GaugeChart, CanvasRenderer, TooltipComponent, TitleComponent]);

// Datos actualizados
const segments = [
  {
    value: 65, // porcentaje de uso de memoria
    name: "Uso de Memoria (%)",
    unit: "%",
    color: "#f59e0b",
  },
  {
    value: 2.5, // tiempo de carga real en segundos
    name: "Tiempo de Carga (s)",
    unit: "s",
    color: "#10b981",
  },
];

const chartOptions = computed(() => ({
  title: {
    text: "Relación: Uso de Recursos vs Carga Inicial",
    padding: 20,
    textStyle: {
      color: "#8C8C8C",
      fontWeight: "bolder",
      fontSize: 15,
    },
    subtext: "Reducir memoria y CPU ayuda a mejorar el tiempo de carga (<3s)",
    subtextStyle: {
      color: "#aaaaaa",
      fontSize: 12,
    },
  },
  series: [
    {
      type: "gauge",
      startAngle: 90,
      endAngle: -270,
      pointer: { show: false },
      progress: {
        show: true,
        overlap: false,
        roundCap: true,
        clip: false,
        itemStyle: {
          borderWidth: 1,
          borderColor: "#464646",
        },
      },
      axisLine: {
        lineStyle: {
          width: 20,
          color: [[1, "#2d3748"]],
        },
      },
      splitLine: { show: false },
      axisTick: { show: false },
      axisLabel: { show: false },
      title: {
        fontSize: 13,
        color: "#8C8C8C",
      },
      detail: {
        width: 60,
        height: 18,
        fontSize: 16,
        color: "inherit",
        borderColor: "inherit",
        borderRadius: 20,
        borderWidth: 1,
        formatter: (value: number, index: number) => {
          const unit = segments[index].unit;
          return `${value} ${unit}`;
        },
      },
      data: segments.map((s, i) => {
        const total = segments.length;
        const offsetY = total > 1 ? -40 + (i * 80) / (total - 1) : 0;
        return {
          value: s.value,
          name: s.name,
          title: {
            offsetCenter: ["0%", `${offsetY - 10}%`],
          },
          detail: {
            offsetCenter: ["0%", `${offsetY + 15}%`],
            color: "#ffffff",
            formatter: `{value} ${s.unit}`,
          },
          itemStyle: {
            color: s.color,
          },
        };
      }),
    },
  ],
}));
</script>

<style scoped>
.gauge-ring-chart {
  min-height: 300px;
  width: 100%;
}
</style>
