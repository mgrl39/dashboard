<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🎯 KPIs</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>

      <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion
          v-for="item in smartGoals"
          :key="item.id"
          :value="item.id.toString()"
        >
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label
                  ><b>{{ element.letter }}</b> →
                  {{ element.content }}</ion-label
                >
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
      <br />
      <h1 class="ion-padding">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion
          v-for="item in technicalGoals"
          :key="item.id"
          :value="item.id.toString()"
        >
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label
                  ><b>{{ element.letter }}</b> →
                  {{ element.content }}</ion-label
                >
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
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
  IonAccordionGroup,
  IonAccordion,
  IonItem,
  IonLabel,
  IonList,
} from "@ionic/vue";
import { ref } from "vue";

// Definición de la estructura de datos para un objetivo SMART
interface SmartElement {
  letter: string;
  content: string;
}

interface SmartGoal {
  id: number;
  title: string;
  description: string;
  smart: SmartElement[];
}

// Array de objetivos SMART de negocio
const smartGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: "Participación en Votaciones",
    description:
      "Alcanzar un 80% de participación diaria en las votaciones de canciones en los próximos 3 meses, implementando notificaciones y recordatorios para aumentar el engagement de los estudiantes.",
    smart: [
      {
        letter: "S",
        content: "Aumentar la participación diaria en votaciones",
      },
      { letter: "M", content: "Alcanzar el 80% de participación" },
      {
        letter: "A",
        content: "Implementando sistema de notificaciones y recordatorios",
      },
      {
        letter: "R",
        content:
          "Para mejorar el engagement y la experiencia musical del centro",
      },
      { letter: "T", content: "En los próximos 3 meses" },
    ],
  },
  {
    id: 2,
    title: "Crecimiento de Usuarios Activos",
    description:
      "Aumentar el número de usuarios activos en un 10% mensualmente durante el próximo semestre, mediante la implementación de un sistema de recompensas y gamificación.",
    smart: [
      { letter: "S", content: "Incrementar usuarios activos mensuales" },
      { letter: "M", content: "10% de incremento mensual" },
      {
        letter: "A",
        content: "Implementando sistema de recompensas y gamificación",
      },
      { letter: "R", content: "Para mantener el compromiso de la comunidad" },
      { letter: "T", content: "Durante el próximo semestre" },
    ],
  },
  {
    id: 3,
    title: "Logros de Usuario",
    description:
      "Lograr que el 70% de los usuarios tengan al menos 3 logros en los próximos 2 meses, mediante la introducción de nuevos desafíos semanales y recompensas especiales.",
    smart: [
      { letter: "S", content: "Aumentar usuarios con 3 o más logros" },
      { letter: "M", content: "70% de usuarios con 3+ logros" },
      {
        letter: "A",
        content: "Introduciendo desafíos semanales y recompensas",
      },
      {
        letter: "R",
        content: "Para incrementar la participación y engagement",
      },
      { letter: "T", content: "En los próximos 2 meses" },
    ],
  },
  {
    id: 4,
    title: "Diversidad Musical",
    description:
      "Mantener una diversidad de géneros musicales donde ningún género supere el 30% de las votaciones totales durante el curso escolar, mediante la implementación de cuotas por género.",
    smart: [
      { letter: "S", content: "Mantener diversidad de géneros musicales" },
      { letter: "M", content: "Ningún género debe superar el 30%" },
      { letter: "A", content: "Implementando sistema de cuotas por género" },
      {
        letter: "R",
        content: "Para asegurar variedad musical y satisfacción general",
      },
      { letter: "T", content: "Durante todo el curso escolar" },
    ],
  },
  {
    id: 5,
    title: "Conexiones Sociales",
    description:
      "Conseguir que el 60% de los usuarios sigan a otros usuarios en la plataforma en los próximos 4 meses, mediante la implementación de sugerencias de usuarios y destacando perfiles activos.",
    smart: [
      { letter: "S", content: "Aumentar conexiones entre usuarios" },
      { letter: "M", content: "60% de usuarios siguiendo a otros" },
      {
        letter: "A",
        content: "Implementando sugerencias y destacando perfiles activos",
      },
      { letter: "R", content: "Para crear una comunidad más conectada" },
      { letter: "T", content: "En los próximos 4 meses" },
    ],
  },
]);

// Array de objetivos SMART técnicos
const technicalGoals = ref<SmartGoal[]>([
  {
    id: 6,
    title: "Tiempo de Respuesta del Servidor",
    description:
      "Monitorizar el tiempo de respuesta del servidor y mantenerlo por debajo de los 200 ms en el 95% de las peticiones durante los próximos 2 meses.",
    smart: [
      { letter: "S", content: "Reducir el tiempo de respuesta del servidor" },
      {
        letter: "M",
        content: "Por debajo de los 200 ms en el 95% de las peticiones",
      },
      {
        letter: "A",
        content: "Optimizando el backend y aplicando cache local",
      },
      {
        letter: "R",
        content: "Para mejorar la experiencia de usuario",
      },
      { letter: "T", content: "En los próximos 2 meses" },
    ],
  },
  {
    id: 7,
    title: "Errores de Autenticación",
    description:
      "Reducir los errores de autenticación un 20% mensual mediante mejoras en el sistema de login y detección temprana de errores.",
    smart: [
      { letter: "S", content: "Reducir errores de autenticación" },
      { letter: "M", content: "Reducción del 20% mensual" },
      {
        letter: "A",
        content: "Implementando logs detallados y alertas automáticas",
      },
      {
        letter: "R",
        content: "Para garantizar accesos seguros y sin fallos",
      },
      { letter: "T", content: "Durante cada mes" },
    ],
  },
  {
    id: 8,
    title: "Uso de Memoria y CPU",
    description:
      "Mantener el uso de CPU y memoria por debajo del 70% de la capacidad máxima durante todo el trimestre mediante optimización del frontend.",
    smart: [
      { letter: "S", content: "Reducir el uso de memoria y CPU" },
      { letter: "M", content: "Mantener por debajo del 70%" },
      {
        letter: "A",
        content: "Optimizando recursos y limitando procesos innecesarios",
      },
      {
        letter: "R",
        content: "Para asegurar estabilidad del sistema",
      },
      { letter: "T", content: "Durante todo el trimestre" },
    ],
  },
  {
    id: 9,
    title: "Tasa de Éxito en Votaciones",
    description:
      "Lograr una tasa de éxito del 98% en las votaciones completadas respecto a las iniciadas durante los próximos 3 meses.",
    smart: [
      {
        letter: "S",
        content: "Aumentar la tasa de éxito de las votaciones completadas",
      },
      { letter: "M", content: "98% de éxito en transacciones" },
      {
        letter: "A",
        content: "Detectando y corrigiendo errores de envío en frontend",
      },
      {
        letter: "R",
        content: "Para garantizar la fiabilidad del sistema de votación",
      },
      { letter: "T", content: "En los próximos 3 meses" },
    ],
  },
  {
    id: 10,
    title: "Tiempo de Carga de la Aplicación",
    description:
      "Mantener el tiempo de carga inicial de la aplicación por debajo de 3 segundos en el 90% de las sesiones durante los próximos 2 meses.",
    smart: [
      { letter: "S", content: "Reducir el tiempo de carga inicial" },
      { letter: "M", content: "Menos de 3 segundos en el 90% de sesiones" },
      {
        letter: "A",
        content: "Mediante carga diferida de componentes y recursos",
      },
      {
        letter: "R",
        content: "Para mejorar el acceso rápido a la app",
      },
      { letter: "T", content: "Durante los próximos 2 meses" },
    ],
  },
]);
</script>

<style scoped>
ion-accordion.accordion-collapsing ion-item[slot="header"],
ion-accordion.accordion-collapsed ion-item[slot="header"] {
  --background: var(--ion-color-light);
  --color: var(--ion-color-light-contrast);
}

ion-accordion.accordion-expanding ion-item[slot="header"],
ion-accordion.accordion-expanded ion-item[slot="header"] {
  --background: rgba(var(--ion-color-primary-rgb), 0.14);
  color: var(--ion-color-primary);
}
</style>
