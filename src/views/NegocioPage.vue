<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🟢 Fila 1: 4 Columnas -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkDataClicks" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkDataViews" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkDataLeads" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box">
              <spark-line v-bind="sparkDataSales" />
            </div>
          </ion-col>
        </ion-row>

        <!-- �� Fila 2: 2 Columnas (antes era fila 3) -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <EchartsMap
                :data="dataDownloadsEU"
                title="Descargas UE"
                subtitle="KPI: 8 Países > 1K"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="6">
            <div class="box">Columna 2</div>
          </ion-col>
        </ion-row>

        <!-- 🟠 Fila 3: 2 Columnas (antes era fila 2) -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGauge :value="dataGauge" title="DESCARGAS" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="9">
            <div class="box">
              <ApexMixedChart :series="dataMixedChartSeries" />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons,
  IonContent,
  IonHeader,
  IonMenuButton,
  IonPage,
  IonTitle,
  IonToolbar,
  IonGrid,
  IonRow,
  IonCol,
} from "@ionic/vue";
import SparkLine from "@/components/SparkLine.vue";
import ApexMixedChart from "@/components/ApexMixedChart.vue";
import EchartsGauge from "@/components/EchartsGauge.vue";
import EchartsMap from "@/components/EchartsMap.vue";
import { ref } from "vue";

// SparkLine data for Clicks (blue)
const sparkDataClicks = ref({
  title: "CLICKS",
  value: "1234",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "navigate-outline",
  chartOptions: {
    chart: {
      id: "clicks",
      type: "area",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "" } },
    },
  },
  chartSeries: [{ data: [25, 66, 41, 59, 25, 44, 12, 36, 9, 21] }],
});

// SparkLine data for Views (lilac)
const sparkDataViews = ref({
  title: "VIEWS",
  value: "1982",
  bgColor: "gradient-purple",
  textColor: "white",
  iconName: "eye-outline",
  chartOptions: {
    chart: {
      id: "views",
      type: "area",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "" } },
    },
  },
  chartSeries: [{ data: [35, 41, 62, 42, 13, 18, 29, 37, 36, 51] }],
});

// SparkLine data for Leads (orange)
const sparkDataLeads = ref({
  title: "LEADS",
  value: "2011",
  bgColor: "gradient-orange",
  textColor: "white",
  iconName: "people-outline",
  chartOptions: {
    chart: {
      id: "leads",
      type: "area",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "" } },
    },
  },
  chartSeries: [{ data: [41, 29, 35, 42, 19, 32, 45, 22, 31, 38] }],
});

// SparkLine data for Sales (green)
const sparkDataSales = ref({
  title: "SALES",
  value: "€627K",
  bgColor: "gradient-green",
  textColor: "white",
  iconName: "cash-outline",
  chartOptions: {
    chart: {
      id: "sales",
      type: "area",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "" } },
    },
  },
  chartSeries: [{ data: [18, 30, 36, 32, 40, 26, 35, 40, 38, 46] }],
});

// 📊 Data: ApexMixedChart
const dataMixedChartSeries = ref([
  {
    name: "Column",
    type: "column",
    data: [23, 11, 22, 27, 13, 22, 37, 21, 44, 22, 30],
  },
  {
    name: "Area",
    type: "area",
    data: [44, 55, 41, 67, 22, 43, 21, 41, 56, 27, 43],
  },
  {
    name: "line",
    type: "line",
    data: [30, 25, 36, 30, 45, 35, 64, 52, 59, 36, 39],
  },
]);

// 🧭 Data: EchartsGauge
const dataGauge = ref(70);

// 🌎 Data: EchartsMap - GEOMAPS
const dataDownloadsEU = ref([
  { name: "Germany", value: 15000 },
  { name: "France", value: 12000 },
  { name: "Spain", value: 16000 },
  { name: "Italy", value: 9000 },
  { name: "Netherlands", value: 8000 },
  { name: "Poland", value: 7500 },
  { name: "Portugal", value: 3000 },
]);
</script>

<style scoped>
ion-row {
  overflow: hidden;
  /*border: 1px solid red;*/
}

ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
  /*background-color: blue;*/
}

/* El contenido real de cada columna */
.box {
  background: #1e1e1e;
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {
  ion-grid {
    height: 100%;
  }
  .ion-row-1 {
    height: 20%;
    max-height: 20%;
  }
  .ion-row-2 {
    height: 40%;
    max-height: 40%;
  }
  .ion-row-3 {
    height: 40%;
    max-height: 40%;
  }
}
</style>
