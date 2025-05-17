<template>
  <VEChart class="map-chart" :option="mapOptions" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect, onMounted } from "vue";

// Echarts
import VEChart from "vue-echarts";
import * as echarts from "echarts/core";
import { MapChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
import {
  TooltipComponent,
  VisualMapComponent,
  TitleComponent,
  ToolboxComponent,
} from "echarts/components";

// GeoMap
import europeMap from "@/assets/europe.geo.json";

// Registrar módulos necesarios de ECharts
echarts.use([
  MapChart,
  CanvasRenderer,
  TooltipComponent,
  VisualMapComponent,
  TitleComponent,
  ToolboxComponent,
]);

// Props
const props = withDefaults(
  defineProps<{
    title?: string;
    subtitle?: string;
    data: { name: string; value: number }[];
  }>(),
  {
    title: "Título del gráfico",
    subtitle: "Subtítulo",
    data: () => [],
  }
);

// 🌎 Configuración del gráfico
const mapOptions = ref({});

watchEffect(() => {
  mapOptions.value = {
    title: {
      text: "DESCARGAS UE",
      subtext: "KPI: 8 Países > 1K",
      left: "left",
      textStyle: {
        color: "#8C8C8C",
        fontSize: 16,
        fontWeight: "bold",
      },
      subtextStyle: {
        color: "#8C8C8C",
      },
    },
    toolbox: {
      show: true,
      left: "right",
      top: "top",
      feature: {
        dataView: {
          readOnly: false,
          backgroundColor: "#1E1E1E",
          textareaColor: "#1E1E1E",
          textColor: "#8C8C8C",
          buttonColor: "#071c49",
          lang: ["Datos", "Cerrar", "Actualizar"],
        },
        restore: {},
        saveAsImage: {},
      },
    },
    tooltip: {
      trigger: "item",
    },
    visualMap: {
      min: 3000,
      max: 16000,
      left: "left",
      top: "bottom",
      text: ["Más", "Menos"],
      textStyle: { color: "#B9B8CE" },
      calculable: true,
      inRange: { color: ["#e0f3f8", "#74add1", "#4575b4"] }, // 🎨 De claro a oscuro
      outOfRange: {
        color: ["rgba(200, 200, 200, 0.2)"], // Color para países sin datos (gris transparente)
      },
    },
    series: [
      {
        name: "Descargas",
        type: "map",
        map: "europe",
        roam: true, // 🖱️ Permitir zoom y desplazamiento
        emphasis: {
          label: { show: true },
          itemStyle: {
            areaColor: "#74add1",
            borderWidth: 1,
          },
        },
        itemStyle: {
          // Color por defecto para países sin datos
          areaColor: "rgba(128, 128, 128, 0.5)", // Gris muy transparente
          borderColor: "rgba(255, 255, 255, 0.5)",
          borderWidth: 0.5,
        },
        data: props.data, // Conectar datos con el mapa
      },
    ],
  };
});

// Registrar el mapa de Europa en ECharts juntando Iceland en el mapa
onMounted(() => {
  echarts.registerMap("europe", europeMap as any, {
    Iceland: {
      left: -20,
      top: 65,
      width: 12,
    },
  });
});
</script>

<style scoped>
.map-chart {
  height: 100%;
  min-height: 350px;
  width: 100%;
}
</style>
