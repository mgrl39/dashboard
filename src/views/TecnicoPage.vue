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
        <!-- 🟢 Fila 1: 3 Columnas -->
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

        <!-- 🔵 Fila 2: 2 Columnas -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="3" push-md="9">
            <div class="box">
              <EchartsGauge :value="currentValue" title="USUARIOS ONLINE" />
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

        <!-- 🟠 Fila 3: 3 Columnas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="12">
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

// Sparkline
import SparkLine from "@/components/SparkLine.vue";

// ApexLineChart Realtime
import ApexLineRT from "@/components/ApexLineRT.vue";

// Echarts
import EchartsGauge from "@/components/EchartsGauge.vue";
import EchartsGaugeMultiple from "@/components/EchartsGaugeMultiple.vue";

/***** 🛠️ CONSTANTES y VARIABLES *****/

// Constantes
const UPDATE_INTERVAL = 1000;
const MAX_DATA_POINTS = 60; // Límitar nº puntos del gráfico para mejor rendimiento (ApexLineRT)

// Variables
let lastDate = Date.now();
let interval: ReturnType<typeof setInterval>;

/***** 🗃️ DATA *****/

// Data: SparkLine
const sparkData1 = ref({
  title: "TIEMPO RESPUESTA",
  value: "178ms",
  bgColor: "gradient-blue",
  textColor: "white",
  iconName: "timer-outline",
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
  title: "CPU USAGE",
  value: "65%",
  bgColor: "gradient-pink",
  textColor: "white",
  iconName: "hardware-chip-outline",
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
  iconName: "checkmark-circle-outline",
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

// Data: ApexLineRT - Series
const data = ref<{ x: number; y: number }[]>([]);
const series = ref([{ name: "Usuarios", data: data.value }]);

// Data: ECharts - Gauge Simple: Valor inicial
const currentValue = ref(0);

// Data: Echarts - Gauge Múltiple: Valores iniciales
const ringSegments = ref([
  { value: 0, name: "🥘 España", color: "#f97316", min: 80, max: 99 },
  { value: 0, name: "🌍 Mundo", color: "#10b981", min: 10, max: 30 },
]);

/***** 🧠 LÓGICA REALTIME *****/
function addDataRealTime() {
  const newX = lastDate + UPDATE_INTERVAL;
  const newY = Math.floor(Math.random() * 90) + 10;
  data.value.push({ x: newX, y: newY });

  // ApexLineRT - Cuidar uso de memoria
  if (data.value.length > MAX_DATA_POINTS) {
    data.value = data.value.slice(-2);
    series.value = [{ name: "Usuarios", data: data.value }]; // Asegúra que ApexCharts recoja el cambio
  }

  lastDate = newX;

  // ECharts - Gauge simple: nuevo valor
  currentValue.value = newY;

  // Echarts - Gauge múltiple: nuevo valor para cada segmento
  ringSegments.value.forEach((s) => {
    s.value = Math.floor(Math.random() * (s.max - s.min + 1)) + s.min;
  });
}

/***** 🔄 CICLO DE VIDA *****/
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

/* El contenido real de cada columna */
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

/* Estilos base para el contenido */
ion-content {
  --background: #38101a;
  --color: #f4f4f4;
}

/* Estilos para el header */
ion-header {
  background: #38101a;
}

ion-toolbar {
  --background: #38101a;
  --color: #f4f4f4;
}

/* Estilos para los botones del menú */
ion-menu-button {
  --color: #f4f4f4;
}

/* Estilos para el título */
ion-title {
  color: #f4f4f4;
}

/* Aplicar altura total y por filas, solo en pantallas ≥ lg */
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
