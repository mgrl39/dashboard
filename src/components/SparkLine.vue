<template>
  <div :class="['box-sparkline', bgColor, textColor]">
    <div class="details">
      <div>
        <ion-icon :name="iconName"></ion-icon>
        <span>{{ title }}</span>
      </div>
      <span>{{ value }}</span>
    </div>
    <vapexChart
      class="sparkline-chart"
      :height="chartHeight"
      :options="chartOptions"
      :series="chartSeries"
    >
    </vapexChart>
  </div>
</template>

<script setup lang="ts">
import { IonIcon } from "@ionic/vue";
import { addIcons } from "ionicons";
import {
  navigateOutline,
  logoIonic,
  eyeOutline,
  peopleOutline,
  cashOutline,
  statsChartOutline,
  trophyOutline,
  peopleCircleOutline,
} from "ionicons/icons";
import vapexChart from "vue3-apexcharts";
import { ref, watchEffect, onUnmounted } from "vue";

// 📌 Registrar iconos
addIcons({
  "logo-ionic": logoIonic,
  "navigate-outline": navigateOutline,
  "eye-outline": eyeOutline,
  "people-outline": peopleOutline,
  "cash-outline": cashOutline,
  "stats-chart-outline": statsChartOutline,
  "trophy-outline": trophyOutline,
  "people-circle-outline": peopleCircleOutline,
});

// 📌 Definir Props para datos dinámicos
defineProps({
  title: { type: String, default: "Metrica" },
  value: { type: String, default: "#Value" },
  chartOptions: { type: Object, required: true }, // Se espera un objeto de configuración de ApexCharts
  chartSeries: { type: Array, required: true }, // Se espera un array con los datos de la serie
  bgColor: { type: String, default: "" },
  textColor: { type: String, default: "" },
  iconName: { type: String, default: "logo-ionic" },
});

/******* Control altura gráfico según ancho ********************/

const chartHeight = ref("50%");

// Función que ajusta la altura dinámicamente
const updateChartHeight = () => {
  const width = window.innerWidth;

  if (width < 576) chartHeight.value = "30%"; // Breakpoint xs
  else if (width < 768) chartHeight.value = "40%"; // Breakpoint sm
  else chartHeight.value = "50%"; // Breakpoint md y superiores
};

// Ejecutar al cargar y escuchar cambios en el tamaño de la ventana
watchEffect(() => {
  updateChartHeight();
  window.addEventListener("resize", updateChartHeight);
});

// Limpiar el listener cuando el componente se desmonta
onUnmounted(() => {
  window.removeEventListener("resize", updateChartHeight);
});

/************************************************************ */
</script>

<style scoped>
/* Mobile first */

.box-sparkline {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  width: 100%;
  padding: 16px;
  border-radius: 12px;
  container: box / inline-size;
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.box-sparkline::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0.15;
  background: radial-gradient(
      circle at top right,
      rgba(255, 255, 255, 0.2),
      transparent 60%
    ),
    linear-gradient(45deg, transparent 0%, rgba(255, 255, 255, 0.1) 100%);
  z-index: 1;
}

.box-sparkline:hover {
  transform: translateY(-5px) scale(1.02);
  box-shadow: 0 15px 45px rgba(0, 0, 0, 0.35);
}

.details {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  position: relative;
  z-index: 2;
}

.details > div {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.details > div > ion-icon {
  font-size: 2.2rem;
  --ionicon-stroke-width: 32px;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
  transition: transform 0.3s ease;
}

.box-sparkline:hover .details > div > ion-icon {
  transform: scale(1.1);
}

.details > div > span {
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}

.details > span {
  font-size: 2.9rem;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.25);
  background: linear-gradient(to right, #ffffff, rgba(255, 255, 255, 0.8));
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.sparkline-chart {
  min-width: 50px;
  width: 100%;
  position: relative;
  z-index: 2;
}

/* Siendo más ancho, pasamos dato a la derecha de título */
@container box (width >= 324px) {
  .details {
    flex-direction: row;
    justify-content: space-start;
    align-items: start;
    gap: 16px;
  }

  .details > span {
    font-size: 6cqmax;
  }
  .details > div > ion-icon {
    font-size: 4cqmax;
  }
  .details > div > span {
    font-size: 2cqmax;
  }
}

/* 🖥️ En pantallas grandes (>=lg=992) */
@media (min-width: 992px) {
  @container box (width <= 356px) {
    .details {
      flex-direction: row;
      justify-content: space-start;
      align-items: start;
      gap: 16px;
    }
    .details > span {
      font-size: max(1.7rem, 16cqw);
    }
    .details > div > ion-icon {
      font-size: max(1.7rem, 5cqw);
    }
    .details > div > span {
      font-size: max(0.8rem, 5cqw);
    }
  }
}

/* 🎨 Colores de fondo */
.gradient-blue {
  background-image: linear-gradient(135deg, #38101a 0%, #97092c 100%);
  box-shadow: 0 8px 32px rgba(151, 9, 44, 0.3);
}

.gradient-purple {
  background-image: linear-gradient(
    165deg,
    #561524 0%,
    #38101a 50%,
    #97092c 100%
  );
  box-shadow: 0 8px 32px rgba(86, 21, 36, 0.3);
}

.gradient-orange {
  background-image: linear-gradient(
    45deg,
    #640e22 0%,
    #97092c 50%,
    #561524 100%
  );
  box-shadow: 0 8px 32px rgba(100, 14, 34, 0.3);
}

.gradient-green {
  background-image: linear-gradient(
    to right,
    #38101a 0%,
    #561524 50%,
    #640e22 100%
  );
  box-shadow: 0 8px 32px rgba(56, 16, 26, 0.3);
}

.black {
  color: black;
}
.white {
  color: white;
}
</style>
