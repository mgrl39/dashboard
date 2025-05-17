<template></template>

<script setup lang="ts">
// Echarts
import { use } from "echarts/core";
import VEChart from "vue-echarts";
import { GaugeChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";
import { TooltipComponent } from "echarts/components";

// Registrar los módulos necesarios de ECharts
use([GaugeChart, CanvasRenderer, TooltipComponent]);
</script>

<style scoped></style>
