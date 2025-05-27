<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay">
        <ion-content>
          <ion-list id="inbox-list">
            <div class="logo-container">
              <img
                src="@/assets/PuigSoundsLogo.png"
                alt="PuigSounds Logo"
                class="app-logo"
              />
              <div class="title-container">
                <h1 class="app-title">PuigSounds</h1>
                <ion-note>Institut Puig Castellar</ion-note>
              </div>
            </div>

            <ion-menu-toggle
              :auto-hide="false"
              v-for="(p, i) in appPages"
              :key="i"
            >
              <ion-item
                @click="selectedIndex = i"
                router-direction="root"
                :router-link="p.url"
                lines="none"
                :detail="false"
                class="hydrated"
                :class="{ selected: selectedIndex === i }"
              >
                <ion-icon
                  aria-hidden="true"
                  slot="start"
                  :ios="p.iosIcon"
                  :md="p.mdIcon"
                ></ion-icon>
                <ion-label>{{ p.title }}</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import {
  IonApp,
  IonContent,
  IonIcon,
  IonItem,
  IonLabel,
  IonList,
  IonListHeader,
  IonMenu,
  IonMenuToggle,
  IonNote,
  IonRouterOutlet,
  IonSplitPane,
} from "@ionic/vue";
import {
  rocketOutline,
  rocketSharp,
  pulseOutline,
  pulseSharp,
  speedometerOutline,
  speedometerSharp,
} from "ionicons/icons";
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";

const selectedIndex = ref(0);
const appPages = [
  {
    title: "Negocio",
    url: "/negocio",
    iosIcon: rocketOutline,
    mdIcon: rocketSharp,
  },
  {
    title: "Técnico",
    url: "/tecnico",
    iosIcon: pulseOutline,
    mdIcon: pulseSharp,
  },
  {
    title: "KPIs",
    url: "/kpis",
    iosIcon: speedometerOutline,
    mdIcon: speedometerSharp,
  },
];

const route = useRoute();

/********************************************************************** */
// 🔄 Función para actualizar el `selectedIndex` según la URL actual

const updateSelectedIndex = () => {
  const currentPath = route.path;
  const index = appPages.findIndex((page) => page.url === currentPath);
  if (index !== -1) {
    selectedIndex.value = index;
  }
};

// Ejecutar cuando la app carga
onMounted(updateSelectedIndex);

// Ejecutar cada vez que cambia la ruta
watch(route, updateSelectedIndex);
/********************************************************************** */
</script>

<style scoped>
ion-split-pane {
  --side-max-width: 280px;
}

.logo-container {
  display: flex;
  align-items: center;
  padding: 16px;
  gap: 12px;
}

.app-logo {
  width: 48px;
  height: 48px;
  object-fit: contain;
  filter: drop-shadow(0 0 8px rgba(151, 9, 44, 0.3));
}

.title-container {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.app-title {
  margin: 0;
  font-size: 24px;
  font-weight: 700;
  color: #f4f4f4;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  letter-spacing: 0.5px;
  position: relative;
  overflow: hidden;
}

.app-title::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 2px;
  background: #97092c;
  transform: translateX(-100%);
  animation: slideIn 1.5s ease-out forwards;
}

@keyframes slideIn {
  to {
    transform: translateX(0);
  }
}

ion-menu ion-content {
  --background: #38101a;
}

ion-menu.md ion-content {
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 20px;
  --padding-bottom: 20px;
}

ion-menu.md ion-list {
  padding: 20px 0;
  background: #38101a;
}

ion-menu.md ion-note {
  margin-bottom: 30px;
  color: #f4f4f4;
}

ion-menu.md ion-list-header,
ion-menu.md ion-note {
  padding-left: 10px;
  color: #f4f4f4;
}

ion-menu.md ion-list#inbox-list {
  border-bottom: 1px solid #561524;
}

ion-menu.md ion-list#inbox-list ion-list-header {
  font-size: 22px;
  font-weight: 600;
  min-height: 20px;
  color: #f4f4f4;
}

ion-menu.md ion-list#labels-list ion-list-header {
  font-size: 16px;
  margin-bottom: 18px;
  color: #f4f4f4;
  min-height: 26px;
}

ion-menu.md ion-item {
  --padding-start: 10px;
  --padding-end: 10px;
  border-radius: 4px;
  --background: #38101a;
  --color: #f4f4f4;
}

ion-menu.md ion-item.selected {
  --background: #97092c;
}

ion-menu.md ion-item.selected ion-icon {
  color: #f4f4f4;
}

ion-menu.md ion-item ion-icon {
  color: #f4f4f4;
}

ion-menu.md ion-item ion-label {
  font-weight: 500;
}

ion-menu.ios ion-content {
  --padding-bottom: 20px;
  --background: #38101a;
}

ion-menu.ios ion-list {
  padding: 20px 0 0 0;
  background: #38101a;
}

ion-menu.ios ion-note {
  line-height: 24px;
  margin-bottom: 20px;
  color: #f4f4f4;
}

ion-menu.ios ion-item {
  --padding-start: 16px;
  --padding-end: 16px;
  --min-height: 50px;
  --background: #38101a;
  --color: #f4f4f4;
}

ion-menu.ios ion-item.selected ion-icon {
  color: #f4f4f4;
}

ion-menu.ios ion-item ion-icon {
  font-size: 24px;
  color: #f4f4f4;
}

ion-menu.ios ion-list#labels-list ion-list-header {
  margin-bottom: 8px;
  color: #f4f4f4;
}

ion-menu.ios ion-list-header,
ion-menu.ios ion-note {
  padding-left: 16px;
  padding-right: 16px;
  color: #f4f4f4;
}

ion-menu.ios ion-note {
  margin-bottom: 8px;
}

ion-note {
  display: inline-block;
  font-size: 16px;
  color: #f4f4f4;
}

ion-item.selected {
  --color: #f4f4f4;
  --background: #97092c;
}
</style>
