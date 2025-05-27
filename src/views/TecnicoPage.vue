<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>📈 Técnico</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Técnico</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- Grid principal del Dashboard -->
      <ion-grid class="dashboard-grid">
        <!-- 🔵 Fila 1: ApexLineRT + Gauge -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="3" push-md="9">
            <div class="box">
              <EchartsGauge
                :value="cpuUsage"
                title="USO DE CPU"
                :warning-threshold="60"
                :danger-threshold="70"
                :colors="['#059669', '#eab308', '#dc2626']"
              />
            </div>
          </ion-col>
          <ion-col size="12" size-md="9" pull-md="3">
            <div class="box">
              <ApexLineRT
                :series="series"
                title="Usuarios online"
                :kpi-target="70"
                color="#3b82f6"
              />
            </div>
          </ion-col>
        </ion-row>

        <!-- 🟢 Fila 2: SparkLines -->
        <ion-row class="ion-row-1">
          <ion-col size="12" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData1" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData2" />
            </div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box">
              <spark-line v-bind="sparkData3" />
            </div>
          </ion-col>
        </ion-row>

        <!-- 🟠 Fila 3: Doughnut + ApexLineRT (cambiado el orden) -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="4">
            <div class="box">
              <ChartDoughnut />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="8">
            <div class="box">
              <EchartsGaugeMultiple :segments="ringSegments" />
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
import { ref, onMounted, onUnmounted } from "vue";

import SparkLine from "@/components/SparkLine.vue";
import ApexLineRT from "@/components/ApexLineRT.vue";
import EchartsGauge from "@/components/EchartsGauge.vue";
import EchartsGaugeMultiple from "@/components/EchartsGaugeMultiple.vue";
import ChartDoughnut from "@/components/ChartDoughnut.vue";

const UPDATE_INTERVAL = 1000;
const MAX_DATA_POINTS = 60;

let lastDate = Date.now();
let interval: ReturnType<typeof setInterval>;

const sparkData1 = ref({
  title: "TIEMPO RESPUESTA",
  value: "178ms",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "stats-chart-outline",
  chartOptions: {
    chart: {
      id: "serverResponse",
      type: "area",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "ms" } },
    },
  },
  chartSeries: [{ data: [180, 195, 150, 170, 165, 155, 185, 160, 175, 178] }],
});

const sparkData2 = ref({
  title: "AUTH ERRORS",
  value: "25%",
  bgColor: "gradient-pink",
  textColor: "white",
  iconName: "people-outline",
  chartOptions: {
    chart: {
      id: "cpuUsage",
      type: "bar",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "smooth", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "%" } },
    },
  },
  chartSeries: [{ data: [65, 68, 62, 64, 66, 63, 67, 65, 64, 65] }],
});

const sparkData3 = ref({
  title: "TASA DE ÉXITO",
  value: "98.5%",
  bgColor: "gradient-orange",
  textColor: "white",
  iconName: "trophy-outline",
  chartOptions: {
    chart: {
      id: "successRate",
      type: "line",
      sparkline: { enabled: true },
      dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 },
    },
    stroke: { curve: "straight", width: 3 },
    colors: ["#fff"],
    tooltip: {
      theme: "dark",
      x: { show: false },
      y: { title: { formatter: () => "%" } },
    },
  },
  chartSeries: [
    { data: [98.5, 98.2, 98.7, 98.4, 98.6, 98.3, 98.5, 98.4, 98.6, 98.5] },
  ],
});

const data = ref<{ x: number; y: number }[]>([]);
const series = ref([{ name: "Usuarios", data: data.value }]);

const currentValue = ref(0);
const cpuUsage = ref(0);
const ringSegments = ref([
  { value: 0, name: "🥘 España", color: "#f97316", min: 80, max: 99 },
  { value: 0, name: "🌍 Mundo", color: "#10b981", min: 10, max: 30 },
]);

function addDataRealTime() {
  const newX = lastDate + UPDATE_INTERVAL;
  const newY = Math.floor(Math.random() * 90) + 10;
  data.value.push({ x: newX, y: newY });

  if (data.value.length > MAX_DATA_POINTS) {
    data.value = data.value.slice(-2);
    series.value = [{ name: "Usuarios", data: data.value }];
  }

  lastDate = newX;
  currentValue.value = newY;

  cpuUsage.value = Math.floor(Math.random() * 45) + 40;

  ringSegments.value.forEach((s) => {
    s.value = Math.floor(Math.random() * (s.max - s.min + 1)) + s.min;
  });
}

onMounted(() => {
  interval = setInterval(addDataRealTime, UPDATE_INTERVAL);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<style scoped>
ion-row {
  overflow: hidden;
}

ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
}

.box {
  background: #561524;
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: start;
}

ion-content {
  --background: #38101a;
  --color: #f4f4f4;
}

ion-header {
  background: #38101a;
}

ion-toolbar {
  --background: #38101a;
  --color: #f4f4f4;
}

ion-menu-button {
  --color: #f4f4f4;
}

ion-title {
  color: #f4f4f4;
}

@media (min-width: 992px) {
  ion-grid {
    height: 100%;
  }
  .ion-row-1 {
    height: 20%;
    max-height: 20%;
  }
  .ion-row-2 {
    height: 50%;
    max-height: 50%;
  }
  .ion-row-3 {
    height: 30%;
    max-height: 30%;
  }
}
</style>
