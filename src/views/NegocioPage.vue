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

        <!-- 🟠 Fila 2: 2 Columnas (antes era fila 3) -->
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

// SparkLine data for Daily Participation (blue)
const sparkDataClicks = ref({
  title: "PARTICIPACIÓN",
  value: "78%",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "stats-chart-outline",
  chartOptions: {
    chart: {
      id: "participation",
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
  chartSeries: [{ data: [65, 72, 78, 75, 80, 76, 73, 78, 82, 79] }],
});

// SparkLine data for Active Users (lilac)
const sparkDataViews = ref({
  title: "USUARIOS ACTIVOS",
  value: "+12%",
  bgColor: "gradient-purple",
  textColor: "white",
  iconName: "people-outline",
  chartOptions: {
    chart: {
      id: "activeUsers",
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
  chartSeries: [{ data: [320, 350, 380, 410, 445, 470, 510, 560, 590, 630] }],
});

// SparkLine data for Achievements (orange)
const sparkDataLeads = ref({
  title: "LOGROS",
  value: "65%",
  bgColor: "gradient-orange",
  textColor: "white",
  iconName: "trophy-outline",
  chartOptions: {
    chart: {
      id: "achievements",
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
  chartSeries: [{ data: [45, 48, 52, 55, 58, 62, 63, 65, 67, 65] }],
});

// SparkLine data for Social Connections (green)
const sparkDataSales = ref({
  title: "SEGUIDORES",
  value: "55%",
  bgColor: "gradient-green",
  textColor: "white",
  iconName: "people-circle-outline",
  chartOptions: {
    chart: {
      id: "followers",
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
  chartSeries: [{ data: [40, 42, 45, 48, 50, 52, 54, 55, 55, 55] }],
});

// 📊 Data: ApexMixedChart - Género Musical Distribution
const dataMixedChartSeries = ref([
  {
    name: "Pop",
    type: "column",
    data: [25, 28, 27, 25, 26, 28, 27, 25, 26, 24, 25],
  },
  {
    name: "Rock",
    type: "area",
    data: [22, 24, 23, 25, 22, 20, 21, 23, 24, 25, 23],
  },
  {
    name: "Hip Hop",
    type: "line",
    data: [18, 20, 19, 21, 20, 19, 22, 21, 20, 19, 20],
  },
]);

// 🧭 Data: EchartsGauge - Participación Diaria
const dataGauge = ref(78);

// 🌎 Data: EchartsMap - Participación por Clases
const dataDownloadsEU = ref([
  { name: "1º ESO", value: 85 },
  { name: "2º ESO", value: 82 },
  { name: "3º ESO", value: 78 },
  { name: "4º ESO", value: 75 },
  { name: "1º Bach", value: 70 },
  { name: "2º Bach", value: 68 },
  { name: "Ciclos", value: 72 },
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
